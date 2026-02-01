# Snort lab for IDS and IPS

🎯 Lab Objectives

By the end of this lab, i will be able to:

- Deploy Snort as IDS (passive detection)
- Deploy Snort as IPS (inline prevention)
- Generate attacks and observe alerts vs blocked traffic
- Clearly explain IDS vs IPS in interviews 💼

|   VMM    |        OS           |      Purpose      |
| -------- | ------------------- | ----------------- |
| Attacker | Kali Linux          | Generate attacks  |
| Snort    | Ubuntu Server 22.04 | IDS / IPS         |
| Victim   | Ubuntu Server       | Target web server |

### PART 1️⃣ — Snort as IDS (Detection Only)
1. install snot on to the ubuntu server
     - sudo apt install -y snort
     - snort -V (for verifying the version of snort)
       
<img width="950" height="800" alt="Screenshot 2026-01-31 235503" src="https://github.com/user-attachments/assets/8a605921-1bf8-4d3a-b1ed-1969227b3c34" />
  
2. Configure snort for IDS mode
     - sudo nano /etc/snort/snort.conf
     - ipvar HOME_NET 192.168.56.0/24
     - ipvar EXTERNAL_NET any
     - sudo snort -T -c /etc/snort/snort.conf
         - we will see a massage of 'Snort successfully validated the configuration'
<img width="928" height="765" alt="Screenshot 2026-02-01 022614" src="https://github.com/user-attachments/assets/755d7579-6bdd-4dda-a2cd-747e0f678af6" />

---

<img width="823" height="816" alt="Screenshot 2026-02-01 022723" src="https://github.com/user-attachments/assets/ac1cb209-11a2-4d58-af34-bef27f174b62" />

3. Strart snort in IDS mode
     - sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
         - -A console is used to show alert on the screen
         - -i eth0 is specifying lisetning a traffic running on the interface
4. Generate at attack using kali
   - nmap -sS 192.168.56.6
   - nmap -O 192.168.56.6
Based on the nmap scan, alert report were produced on the screen informing the type of scann was run on the system including the ttype of scan like
SYN scann the massage or alert

<img width="897" height="657" alt="Screenshot 2026-02-01 024104" src="https://github.com/user-attachments/assets/d2b2280a-e1e1-4ec6-9ae3-848c83372cfa" />

---

<img width="981" height="207" alt="Screenshot 2026-02-01 024122" src="https://github.com/user-attachments/assets/82d4d6fc-09c5-4f33-b84d-191187ff600b" />

Based on what we learn so far is that, in the IDS mode, snort will allow traffic in to the system but will create an alert or log of any suspicious traffic with its detail information like the ports and protocolo through which the traffic is passing through including the flags (tcp flags) the source ip address

