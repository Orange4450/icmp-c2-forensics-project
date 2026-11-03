## 2026-11-02
Old VMs removed. Starting fresh environment setup from scratch.

## 2026-11-04
Created victim VM (Ubuntu Desktop 26.04, 4096MB RAM, 25GB disk) and
attacker-c2 VM (Ubuntu Server 26.04, 2048MB RAM, 15GB disk, openssh-server
installed). Both VMs installed and verified working.

## 2026-11-04
Network configured: labnet internal network + NAT adapter on both VMs
(victim and attacker-c2); promiscuous mode (Allow All) enabled on both.