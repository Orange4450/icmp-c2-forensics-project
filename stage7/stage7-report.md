# Final Forensic Report — Detection of Hidden Command-and-Control (C2) Channels Based on ICMP Tunneling

**Student:** Boltabay Zhaniya Kanyshkyzy, group SIS-2313
**Course:** Digital Forensics of Network Infrastructure

## 2. Objective

To study the fundamental principles of digital forensics in network
infrastructure, and to detect and document a hidden command-and-control
(C2) channel based on ICMP tunneling, covering the full investigation
lifecycle from identification through evidence correlation, timeline
reconstruction, and final reporting.

## 3. Investigation Scenario

A workstation in a training network ("victim", 192.168.100.10) is
suspected of being compromised and maintaining a hidden C2 channel with
an external node ("attacker-c2", 192.168.100.20) via ICMP tunneling,
bypassing a perimeter policy that blocks outbound TCP on non-standard
ports but allows ICMP.

*[MODELED CONTEXT - for educational purposes only, not derived from
captured traffic]* No dedicated firewall device exists in the lab
topology. A plausible perimeter policy is modeled as: outbound TCP on
non-standard ports is denied, while ICMP is allowed - providing the
scenario's motive for the attacker's choice of ICMP as a covert channel.

## 4. Network Environment

| Host | Role | OS | IP Address | Interface |
|---|---|---|---|---|
| victim | Compromised workstation (simulated) | Ubuntu Desktop (GUI) | 192.168.100.10/24 | enp0s3 |
| attacker-c2 | C2 server (simulated) | Ubuntu Server (headless) | 192.168.100.20/24 | enp0s3 |

Both VMs connected via an isolated VirtualBox Internal Network
("labnet"), with promiscuous mode set to "Allow All". A second NAT
adapter on each host is used only for package installation and is not
involved in the simulated C2 traffic.

Tools used: Oracle VirtualBox (lab environment), Wireshark (traffic
capture and analysis), ptunnel-ng (ICMP tunneling utility used to
generate the simulated C2 traffic), OpenSSH client (application-layer
traffic carried inside the tunnel), sha256sum (evidence integrity
verification), git/GitHub (process documentation and evidence
versioning).

## 5. Data and Sources of Digital Evidence

See Stage 1, Task 1 (stage1/stage1-report.md): primary sources are the
victim workstation (network traffic, process list, system/auth logs) and
the attacker-c2 server (network traffic, process list, system logs). No
router, firewall, or switch device exists in this lab topology.

## 6. Evidence Collection and Preservation

A single ICMP-tunneled SSH session was captured on victim (interface
enp0s3) using Wireshark and saved as LAB1_Network_Evidence.pcapng.
SHA-256 hash: a3c7a8fed09b3db88984655271fa61782e22ca7cf2ad1c025e085c1c26bdeac6
(Stage 1, Task 5). The file was transferred to the project repository
(stage1/evidence/) via a GitHub Personal Access Token, with the hash
re-verified unchanged before and after transfer, and a Chain of Custody
maintained throughout (Stage 1, Task 6).

## 7. Network Traffic Analysis

See Stage 2 (stage2/stage2-report.md). Key finding: a single ICMP
"conversation" (id=0xaeb1) of 3,918 packets between victim and
attacker-c2, with highly irregular packet lengths (70-1094 bytes). Large
packets' Data field contains readable SSH key-exchange protocol strings
preceded by a "deadc0de" marker, confirming SSH traffic is tunneled
inside ICMP Echo request/reply packets.

## 8. Log and Artifact Analysis

See Stage 3 (stage3/stage3-report.md). System logs (journalctl) on both
hosts confirm sudo-elevated execution of ptunnel-ng with matching
command-line arguments: server started on attacker-c2 at 2026-11-19
21:37:53 UTC, followed by the client on victim at 2026-11-19 21:39:42
UTC (~2 minutes later). No clock drift was found on either host.

## 9. Suspicious Activity and IoCs

See Stage 4 (stage4/stage4-report.md). IoCs: oversized/anomalous ICMP
Echo payload; sustained bidirectional ICMP exchange between a fixed host
pair; a purpose-built ICMP-tunneling utility run with elevated
privileges; correct temporal ordering of C2 setup (server, then client).

## 10. Correlation of Digital Evidence

See Stage 5 (stage5/stage5-report.md). Network-level evidence (Stage 2)
and host-level log evidence (Stage 3) independently corroborate each
other, establishing with high confidence that victim and attacker-c2
communicated via an ICMP-tunneled SSH session, set up in a coordinated,
deliberate manner.

## 11. Incident Timeline

See Stage 6 (stage6/stage6-report.md) for the full timeline table:
server start (21:37:53 UTC) -> client connection (21:39:42 UTC) -> SSH
session tunneled and captured -> evidence saved and hashed -> evidence
transferred to repository (2026-11-22).

## 12. Findings

- A covert C2 channel was established between victim and attacker-c2
  using ICMP tunneling (ptunnel-ng), carrying a complete SSH session.
- The channel is detectable via packet-payload inspection (anomalous
  ICMP size/content) and corroborating host-level logs (sudo-elevated
  process execution, correct temporal sequence).
- The evidence base (PCAP, logs, hashes, chain of custody) is internally
  consistent and supports the findings above with no contradictions.

## 13. Conclusions

This project demonstrated a complete educational digital-forensics
workflow applied to an ICMP-tunneling C2 scenario: identification of
evidence sources, collection and preservation of network evidence,
traffic and log analysis, IoC formulation, evidence correlation, and
incident-timeline reconstruction. The technique (SSH-over-ICMP covert
channel) was successfully detected and documented using standard,
reproducible forensic methods and open-source tools.

## 14. Limitations

- The investigation is conducted in an isolated, student-built lab
  environment; no router, firewall, switch, or IDS/IPS device exists, so
  evidence from these sources is marked "not applicable" rather than
  fabricated.
- The perimeter-firewall policy motivating the attacker's choice of ICMP
  is modeled (Section 3), not derived from captured traffic, and is
  explicitly labeled as such.
- In line with Rule 9 of the project requirements, no claim is made that
  an external attacker compromised the host; the scenario and the C2
  tool were created by the student for this educational exercise.

## Tool Disclosure

An AI assistant (Claude) was used as a supporting tool for formatting
text, command syntax guidance, and documentation structure during this
project. All practical work — network configuration, traffic capture,
log extraction, and analysis — was performed independently by the
student in the VM environment described in this report.

## 15. References

- ptunnel-ng: https://github.com/utoni/ptunnel-ng
- Wireshark: https://www.wireshark.org/
- Project documents: Individual educational project.pdf, Laboratory Work No 1.pdf