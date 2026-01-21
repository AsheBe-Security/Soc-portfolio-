# WIRESHARK LAB — TRAFFIC ANALYSIS & NETWORK VISIBILITY

### 🎯 Lab Objectives

By the end of this lab, you will be able to:

- Capture live traffic between Kali ↔ Ubuntu
- Identify Nmap scan signatures
- Understand TCP handshakes, RSTs, SYNs
- Detect service enumeration & credential exposure
- Think like blue team + red team
  
### 🧱 LAB ARCHITECTURE
For this lab we will use both ubuntu sever and windows guest as a target and kali as attack machine. Ubuntu server will be running important services like HTTP/HTTPS, SSH and FTP. 
Wireshark will be used for capturing and analyzing packets

**Identify the right interface**
- ip a
- eth0 (the interface selected)
  
Run a clean wireshark
 - wireshark &
 - select eth0
1. SSH connection analysis
- From kali try to SSH to the ubuntu server
- ssh ubuntu@192.168.56.6
- filter the ssh traffic
  - tcp.port == 22
- the three way handshake were established by checking on the three tcp flags
    - SYN
    - SYN-ACK
    - ACK
- encreypted password exchange were also observed on the capture packet

  
