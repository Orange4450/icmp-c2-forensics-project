## ICMP C2 Forensics Project

**Course:** Digital Forensics of Network Infrastructure

**Topic:** Detection of Hidden Command-and-Control (C2) Channels Based on ICMP Tunneling

**Student:** Boltabay Zhaniya Kanyshkyzy, group SIS-2313

## Scenario
A workstation in a training network ("victim", 192.168.100.10) is suspected of being
compromised and maintaining a hidden C2 channel with an external node ("attacker-c2",
192.168.100.20) via ICMP tunneling, bypassing a perimeter policy that blocks outbound
TCP on non-standard ports but allows ICMP.

## Environment
- Oracle VirtualBox, two VMs connected via Internal Network "labnet"
- victim: Ubuntu Desktop, 192.168.100.10/24
- attacker-c2: Ubuntu Server (headless), 192.168.100.20/24
- Tools: Wireshark, ptunnel-ng, OpenSSH client, sha256sum

## Structure
- `stage1/` ... `stage7/` — evidence, reports and screenshots per project stage
- `vm-setup/` — network configs and environment setup notes
