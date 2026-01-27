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
   
<img width="872" height="715" alt="Screenshot 2026-01-21 061240" src="https://github.com/user-attachments/assets/05ad17dd-5787-47c7-a352-8a993bfeb625" />
   
1. SSH connection analysis
- From kali try to SSH to the ubuntu server
- ssh ubuntu@192.168.56.6
  
<img width="697" height="497" alt="Screenshot 2026-01-21 061339" src="https://github.com/user-attachments/assets/0f5596c5-89fa-45b2-adf9-eba887a265be" />

- filter the ssh traffic
  - tcp.port == 22
    
 <img width="950" height="791" alt="Screenshot 2026-01-21 061427" src="https://github.com/user-attachments/assets/5446c665-bacf-4019-97e5-dfca8281bc6e" />
 
- the three way handshake were established by checking on the three tcp flags
    - SYN
    - SYN-ACK
    - ACK

2. ftp traffic analysis
  - wireshark was luanched on the common interface
     - wireshark &
  - ftp connection was established with ubuntu server
    - ftp 192.168.56.6
    - 
   <img width="882" height="325" alt="Screenshot 2026-01-21 174018" src="https://github.com/user-attachments/assets/22a2ae26-7452-4005-9d13-f5635f305c5a" />
 
  - the captured packet was analayzed by comparing with the ideal tcp three ways handshake
     - all the three tcp flags (SYN, SYN/ACK, ACK) were observed
  - since ftp doesn't provide encryption, credentials were seen in plain text both login name and password
  - for the sake of excrcise, wrong credentials were provided to see how packet behavs, we have seen a lot of failed login packets, which might suggest about brute force attack.

<img width="942" height="821" alt="Screenshot 2026-01-21 173946" src="https://github.com/user-attachments/assets/8890a4a9-6782-4020-8f8d-a1540c70ed9e" />

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
      
<img width="1267" height="82" alt="Screenshot 2026-01-21183954" src="https://github.com/user-attachments/assets/0a1b3ead-32e0-4b82-81d3-3298a9c27206" />
    
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
    - Page content - (HTML)information about the web server was left in plain text including html and its entire content. if credentials were included, it would be left on plain text.
      
<img width="1278" height="606" alt="Screenshot 2026-01-27 040540" src="https://github.com/user-attachments/assets/cf29b3eb-f7b6-4bbe-9341-346e6baa1374" />

<img width="842" height="437" alt="Screenshot 2026-01-21 185128" src="https://github.com/user-attachments/assets/cd38e699-a138-4b87-8b9e-93014bea8e91" />

<img width="851" height="397" alt="Screenshot 2026-01-21 185059" src="https://github.com/user-attachments/assets/4d6251e5-9529-4c44-b8db-fb5179726577" />

### part tw0 - https traffic
 - curl https://<ubuntu-ip> -k




