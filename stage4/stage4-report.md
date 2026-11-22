# Stage 4 — Indicators of Compromise and Justification

## Table of Suspicious Activity / IoCs

| Indicator | Description | Evidence Source |
|---|---|---|
| Oversized / anomalous ICMP Echo payload | ICMP Echo packets up to 1094 bytes, far larger than the 70-278 byte baseline seen in the same capture, carrying readable SSH key-exchange protocol strings (preceded by a "deadc0de" marker) in the Data field | Stage 2, packet #9 analysis |
| Sustained bidirectional ICMP exchange between a fixed host pair | A single continuous ICMP "conversation" (same id=0xaeb1) of 3,918 packets between victim and attacker-c2, rather than a normal short diagnostic ping | Stage 2, Conversations statistics |
| Purpose-built ICMP-tunneling utility run with elevated privileges | sudo-logged execution of ptunnel-ng on both victim (client) and attacker-c2 (server), with matching command-line arguments and correct server-then-client sequence | Stage 3, journalctl log entries |
| Correct temporal ordering of C2 setup | attacker-c2 server start (2026-11-19 21:37:53 UTC) precedes victim client launch (2026-11-19 21:39:42 UTC) by ~2 minutes, consistent with a coordinated C2 setup rather than coincidental activity | Stage 3, timestamp correlation |

## Justification of Findings

The traffic between 192.168.100.10 (victim) and 192.168.100.20
(attacker-c2) is assessed as suspicious on the basis of: (1) an ICMP
payload size and content inconsistent with normal diagnostic use, directly
observed in Stage 2; (2) the presence of a purpose-built ICMP-tunneling
utility (ptunnel-ng) running with elevated privileges on both endpoints,
confirmed against system logs in Stage 3; (3) a short, low-volume,
single-host-pair ICMP conversation rather than routine background pinging,
consistent with a deliberate, purpose-driven connection rather than
ordinary network noise; and (4) the correct temporal ordering of server
and client launches, consistent with a coordinated covert-channel setup.
No claim is made that an external attacker compromised the host - the
scenario and the C2 tool were created by the student for this educational
exercise; the findings demonstrate that the technique is detectable using
the artifacts listed above.

## Preliminary Investigation Conclusions (Stages 1-4)

Stages 1-4 establish a defined investigation scenario (ICMP-tunneling-based
covert C2 channel between victim and attacker-c2), a primary evidence base
(a hashed, chain-of-custody-tracked PCAP file), traffic-analysis results
identifying the anomalous ICMP packets as the central artifact, log-based
confirmation of the responsible process and its execution order, and a
complete IoC set. These results form the basis for Stage 5 (evidence
correlation), Stage 6 (incident timeline), and Stage 7 (final
comprehensive report).