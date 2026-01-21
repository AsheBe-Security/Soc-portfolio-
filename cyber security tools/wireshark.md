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
2. ftp traffic analysis
  - wireshark was luanched on the coomon interface
     - wireshark &
  - ftp connection was established with ubuntu server
    - ftp 192.168.56.6
  - the captured packet was analayzed by comparing with the ideal tcp three ways handshake
     - all the three tcp flags (SYN, SYN/ACK, ACK) were observed
  - since ftp doesn't provide encryption, credentials were seen in plain text both login name and password
  - for the sake of excrcise, wrong credentials were provided to see how packet behavs, we have seen a lot of failed login packets, which might suggest about brute force attack.


# WIRESHARK LAB — HTTP vs HTTPS (CLEAR TEXT vs ENCRYPTED)
   
  🎯 Objective

on this lab i will be able to:

- HTTP credentials & data in plain text
- HTTPS data as encrypted TLS blobs
- Exactly what defenders and attackers can / cannot see
### perst one - http
1. make sure both http and https running on ubuntu server
    - sudo ss -tulpn | grep apache
    - it showed both listening on thier respevtive ports
2. start a wireshark clean capture
    - wireshark &
    - select the right interface (eth0)
3. send http request either from the terminal or using a broswer of the ubuntu server
    - curl http://192.168.56.6
    - analyze the captured packets of its behaviors, get and response achieved
    - packets were not encrypted, we can see
    - GET / HTTP/1.1
    - Host header
    - User-Agent
    - Full request path
    - Server response
    - Page content - (HTML)information about the web server was left in plain text including html and its entire content. if credentials were included, it would be left on plain text

### part tw0 - https traffic
 - curl https://<ubuntu-ip> -k


