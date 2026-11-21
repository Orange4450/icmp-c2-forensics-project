# Stage 2 — Network Traffic Capture and Analysis Using Network Analysis Tools

Running log of the Stage 2 process: steps taken, commands used, and any
troubleshooting encountered during traffic analysis.

## 2026-11-22

Reopened the Stage 1 evidence file (LAB1_Network_Evidence.pcapng) in
Wireshark on victim (read-only, file unmodified). Applied display filter
`ip.addr==192.168.100.10 && ip.addr==192.168.100.20` to isolate traffic
between the two lab hosts. Initial observation: all ICMP packets share the
same id (0xaeb1), but packet length varies significantly (70, 112, 278,
614, 1094 bytes) across the capture - inconsistent with a normal ping,
where length stays constant. This is a key indicator of ICMP tunneling.

Expanded the ICMP protocol tree for packet #9 (Echo request, id=0xaeb1,
seq=2/512, length 1094 bytes). The Data field is 1052 bytes - far larger
than a normal ICMP echo payload - and begins with a "deadc0de" marker
(likely a ptunnel-ng protocol header), followed by readable SSH
key-exchange algorithm strings (e.g. "diffie-hellman-group-exchange-sha256",
"ecdh-sha2-nistp256"). This confirms SSH protocol data is being carried
inside the ICMP Echo payload - direct evidence of ICMP tunneling.

Checked Statistics → Conversations (IPv4 tab): confirmed only one real
conversation in the capture, 192.168.100.10 <-> 192.168.100.20, 3,918
packets / 416 kB total, duration ~140.67s (the other listed entry,
0.0.0.0 -> 255.255.255.255, is unrelated broadcast noise). This matches
the single ICMP id=0xaeb1 session observed in the packet list, and this
finding is carried forward into Stage 3 (log correlation) and Stage 4
(IoC formulation).