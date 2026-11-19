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

## 2026-11-20

Verified network connectivity (ping victim -> attacker-c2, 0% packet
loss) before proceeding. Re-established the ICMP tunnel: server on
attacker-c2 (./src/ptunnel-ng -r0.0.0.0 -R22), client on victim
(./src/ptunnel-ng -p 192.168.100.20 -l 2222 -r192.168.100.20 -R22).

Started Wireshark capture on victim (enp0s3), then generated tunnel
traffic via `ssh -p 2222 attacker-c2@127.0.0.1` (ran whoami, pwd,
uname -a, ls -la, then exit). SSH session succeeded without issues.
Captured traffic shows a continuous stream of ICMP Echo request/reply
between 192.168.100.10 and 192.168.100.20 with the same id but variable
packet length (70-246 bytes) - a key indicator that the ICMP traffic is
carrying tunneled data rather than being a normal ping.

Stopped the capture and saved the file as LAB1_Network_Evidence.pcapng
on victim's Desktop. Calculated its SHA-256 hash:
a3c7a8fed09b3db88984655271fa61782e22ca7cf2ad1c025e085c1c26bdeac6

Evidence file still needs to be transferred from victim to the
repository (lab1/evidence/).