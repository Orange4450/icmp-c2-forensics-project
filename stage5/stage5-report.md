# Stage 5 — Evidence Correlation and Investigation Results

## Evidence Correlation Table

| Evidence ID | Source / Stage | Description | Correlates With |
|---|---|---|---|
| E-001 | Stage 1 | LAB1_Network_Evidence.pcapng, SHA-256 hashed, chain of custody tracked | Primary evidence file analyzed in all subsequent stages |
| E-001 (packets) | Stage 2 | Single ICMP conversation (id=0xaeb1), 3,918 packets, anomalous payload sizes (up to 1094 bytes) with embedded SSH key-exchange data ("deadc0de" marker) | Confirms covert channel is carrying SSH protocol data |
| Log entry (victim) | Stage 3 | sudo-logged ptunnel-ng client launch, 2026-11-19 21:39:42 UTC | Matches exact command used to generate the Stage 1 capture |
| Log entry (attacker-c2) | Stage 3 | sudo-logged ptunnel-ng server launch, 2026-11-19 21:37:53 UTC | Precedes client launch by ~2 minutes; confirms server-then-client sequence |
| IoC set | Stage 4 | Oversized ICMP payload, sustained single-host-pair conversation, elevated-privilege tunneling tool, correct temporal ordering | Ties network-level and host-level evidence into one coherent incident |

## Description of the Identified Event

The evidence establishes that a covert command-and-control channel was
established between victim (192.168.100.10) and attacker-c2
(192.168.100.20) using ICMP tunneling (ptunnel-ng), carrying an SSH
session inside ICMP Echo request/reply packets. The server-side process
was started first on attacker-c2, followed approximately two minutes
later by the client-side process on victim, after which the tunnel
carried SSH key-exchange traffic. This is consistent with a deliberate,
pre-planned C2 setup rather than incidental or background network
activity.

## Evidence-Based Findings

- The affected host (victim) and the C2 endpoint (attacker-c2) are
  identified with high confidence, based on both network-traffic evidence
  (Stage 2) and host-level log evidence (Stage 3), which independently
  corroborate each other.
- The covert channel's mechanism (ICMP tunneling via ptunnel-ng carrying
  SSH traffic) is identified with high confidence based on direct
  packet-payload inspection (Stage 2).
- The temporal sequence of the C2 setup (server, then client) is
  established with high confidence from system logs (Stage 3) and is
  consistent with the known collection procedure (Stage 1).
- In line with the project's educational scope, no claim is made of an
  external attacker; the scenario and tooling were created by the student
  for this exercise, and the findings demonstrate the technique's
  detectability using standard forensic artifacts.