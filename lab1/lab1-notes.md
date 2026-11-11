# Lab 1 — Fundamentals of Network Digital Forensics and Collection of Network Evidence

Running log of the Lab 1 process: steps taken, commands used, and any
troubleshooting encountered during evidence collection.

## 2026-11-11

Started Wireshark capture setup on victim for Lab 1 evidence collection.
Attempted to start packet capture on interface enp0s3; got "You don't have
permission to capture on local interfaces" despite selecting "Yes" during
Wireshark install. Fixed by running `sudo usermod -aG wireshark $USER` and
`sudo dpkg-reconfigure wireshark-common`, then rebooting victim (had to use
`sudo systemctl reboot -i` due to an active session inhibiting normal
reboot). After reboot, Wireshark capture started successfully on enp0s3.

Re-established the ICMP tunnel (server on attacker-c2, client on victim) and
attempted to generate traffic via `ssh -p 2222 attacker-c2@127.0.0.1` for the
capture. First two attempts failed: ptunnel-ng dropped the tunnel "due to
inactivity" with ~19.5% ICMP packet loss before SSH could complete its
handshake. On a third attempt, plain `ping -c 10 192.168.100.20` from victim
returned 100% packet loss ("Destination Host Unreachable"), even though
`ip a` and `nmcli` on victim showed enp0s3 correctly configured
(192.168.100.10/24, connected).

While diagnosing attacker-c2's network state, both VMs became fully
unresponsive (no reaction to keyboard/mouse). Checked host Task Manager:
RAM usage at 89%, with Microsoft Edge (35 tabs) consuming ~440MB. Likely
cause: host resource exhaustion from running two VMs simultaneously
alongside many browser tabs and VS Code. Resolved by resetting both VMs via
VirtualBox (Machine > Reset) after closing unnecessary host applications.