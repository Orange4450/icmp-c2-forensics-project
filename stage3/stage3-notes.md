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