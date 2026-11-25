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
- Oracle VirtualBox 7.2.4, two VMs connected via Internal Network "labnet"
- victim: Ubuntu Desktop 26.04 LTS, 2 vCPU, 4096 MB RAM, 25 GB disk (VDI), 192.168.100.10/24
- attacker-c2: Ubuntu Server 26.04 LTS (headless), 2 vCPU, 2048 MB RAM, 15 GB disk (VDI), 192.168.100.20/24
- Linux kernel 7.0.0-42-generic, interface enp0s3
- Tools: Wireshark 4.6.4, ptunnel-ng 1.42, OpenSSH 10.2p1, sha256sum

## Structure
- `stage1/` ... `stage7/` — evidence, reports and screenshots per project stage
- `vm-setup/` — network configs and environment setup notes

## Project size
77 files, about 6.8 MB: 16 .md reports (~32 KB), 58 screenshots (~6.2 MB),
LAB1_Network_Evidence.pcapng (~540 KB), 2 netplan configs. No own source code.
