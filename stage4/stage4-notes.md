# Stage 4 — Detection of Suspicious and Anomalous Network Activity

Running log of the Stage 4 process: analysis performed and conclusions
reached while correlating Stage 2 (traffic) and Stage 3 (log) evidence.

## 2026-11-22

Consolidated findings from Stage 2 (anomalous ICMP payload sizes and
embedded SSH key-exchange data) and Stage 3 (sudo-logged ptunnel-ng
launches on both hosts, correct execution order) to formulate a set of
Indicators of Compromise (IoCs) for the ICMP-tunneling C2 scenario. No
additional VM work was required for this stage; all analysis is based on
evidence already collected and documented.