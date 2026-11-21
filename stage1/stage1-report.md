# Stage 1 — Task Tables (Identification, Artifacts, Evidence Record, Chain of Custody)

## Task 1 — Identification of Sources of Digital Evidence

| Component | Potential Digital Evidence |
|---|---|
| User workstation (victim) | Captured network traffic (PCAP/PCAPNG); running-process list (ptunnel-ng client); shell command history; system/auth logs recording sudo execution of the tunneling tool |
| Server (attacker-c2) | Captured/observed network traffic; running-process list (ptunnel-ng server); connection timestamps; system logs |
| Router | Not applicable — no dedicated router device exists in the VirtualBox lab topology |
| Firewall | Not applicable as a physical device in this lab; a modeled firewall policy is referenced in Stage 3 to justify the attacker's choice of ICMP as a covert channel |
| Network switch | Not applicable — VirtualBox Internal Network does not produce switch-level logs |

## Task 2 — Identification of Network Artifacts

| Artifact | Type | Possible Source | Significance for Investigation |
|---|---|---|---|
| IP address | Network identifier | PCAP; VM network configuration | Identification of the communicating hosts (victim / attacker-c2) |
| MAC address | Link-layer identifier | PCAP (Ethernet header) | Associates captured traffic with a specific VM network interface |
| Timestamp | Temporal artifact | PCAP; system logs | Reconstruction of the event timeline; correlation across sources |
| ICMP connection record (id/seq pair) | Session/flow artifact | PCAP | Groups request/reply packets into a single ICMP "session", separating tunnel traffic from control ping |
| ICMP packet with anomalous payload | Protocol-layer artifact | PCAP (Wireshark) | Primary indicator of ICMP tunneling — payload length/content exceeds a normal echo request |
| Network traffic capture file | Raw evidence file | Wireshark (.pcapng) | Primary source of all further evidence; integrity-verified via SHA-256 |

## Task 4 — Evidence Record

| Field | Value |
|---|---|
| Evidence ID | E-001 |
| File Name | LAB1_Network_Evidence.pcapng |
| Data Type | Network traffic capture (PCAPNG) |
| Source | victim (192.168.100.10), interface enp0s3 |
| Date and Time of Collection | 2026-11-20 |
| Collection Method | Wireshark packet capture during an active ICMP tunnel (ptunnel-ng) session carrying SSH traffic |
| Responsible Person | Boltabay Zhaniya Kanyshkyzy |

## Task 6 — Chain of Custody

| Evidence ID | Date and Time | Action | Responsible Person |
|---|---|---|---|
| E-001 | 2026-11-20, capture | Data collection (packet capture started on victim, enp0s3) | Boltabay Zhaniya Kanyshkyzy |
| E-001 | 2026-11-20, immediately after capture | SHA-256 hash calculation (a3c7a8fed09b3db88984655271fa61782e22ca7cf2ad1c025e085c1c26bdeac6) | Boltabay Zhaniya Kanyshkyzy |
| E-001 | 2026-11-22 | Transfer to repository for analysis (via GitHub PAT method on victim; original preserved, hash re-verified before transfer) | Boltabay Zhaniya Kanyshkyzy |