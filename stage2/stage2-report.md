# Stage 2 — Table of Significant Connections

| No. | Source | Destination | Protocol | Total Packets | Total Bytes | Observation |
|---|---|---|---|---|---|---|
| 1 | 192.168.100.10 | 192.168.100.20 | ICMP | 3,918 | 416 kB | Single continuous ICMP "conversation" (same id=0xaeb1) mixing normal-size echo packets (70-278 bytes) with abnormally large ones (614-1094 bytes); large packets' Data field contains readable SSH key-exchange strings — consistent with ptunnel-ng tunneling SSH traffic inside ICMP Echo request/reply |