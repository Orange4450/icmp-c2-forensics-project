## 2026-11-02
Old VMs removed. Starting fresh environment setup from scratch.

## 2026-11-04
Created victim VM (Ubuntu Desktop 26.04, 4096MB RAM, 25GB disk) and
attacker-c2 VM (Ubuntu Server 26.04, 2048MB RAM, 15GB disk, openssh-server
installed). Both VMs installed and verified working.

## 2026-11-04
Network configured: labnet internal network + NAT adapter on both VMs
(victim and attacker-c2); promiscuous mode (Allow All) enabled on both.

## 2026-11-04
Static IPs configured via netplan: 
victim = 192.168.100.10/24,
attacker-c2 = 192.168.100.20/24 (both on enp0s3/labnet interface).

## 2026-11-05
Troubleshooting: ping between victim and attacker-c2 failed after static IP
setup (100% packet loss, tcpdump showed no packets arriving). Verified
identical VirtualBox Internal Network config ('labnet') via VBoxManage on
both VMs - config correct. On attacker-c2, networkctl shows interface fully
online and configured. On victim, NetworkManager shows "Activation of
network connection failed" notification at boot. Diagnosing further.

## 2026-11-05
Root cause found: 00-installer-config.yaml set dhcp4: true on enp0s3,
conflicting with static IP in 50-cloud-init.yaml - NetworkManager tried to
also acquire a DHCP lease (none available on labnet), causing connection
activation timeout/failure. Fix: added "dhcp4: no" explicitly to
50-cloud-init.yaml on victim. Connection now activates successfully
(nmcli: enp0s3 connected via netplan-enp0s3).

## 2026-11-05
Network connectivity confirmed working: ping between victim (192.168.100.10)
and attacker-c2 (192.168.100.20) successful, 0% packet loss.

## 2026-11-11
Installed required forensic tools: Wireshark, git, build-essential, autoconf
on victim; git, build-essential, autoconf on attacker-c2 (no GUI, Wireshark
not needed there). Both installations completed without errors.

## 2026-11-11
Built ptunnel-ng from source (github.com/utoni/ptunnel-ng) on both victim
and attacker-c2 via git clone + autogen.sh + make. Build succeeded on both
VMs without errors; executable src/ptunnel-ng created on each.

## 2026-11-11
Final tunnel verification: started ptunnel-ng server on attacker-c2
(./src/ptunnel-ng -r0.0.0.0 -R22) and client on victim
(./src/ptunnel-ng -p 192.168.100.20 -l 2222 -r192.168.100.20 -R22).
Verified end-to-end by SSH'ing from victim to attacker-c2 through the
tunnel (ssh -p 2222 attacker-c2@127.0.0.1) - connection succeeded,
confirming the ICMP C2 tunnel is fully functional. Environment setup
(Stage 0) is complete. Tunnel left running for use in Lab 1/Lab 2 traffic
capture.

## 2026-11-20
Added a VirtualBox Shared Folder on victim ("evidence", auto-mount) pointing
to the host's lab1/evidence/ directory, to transfer the Lab 1 evidence file
without needing git/token authentication inside the VM.