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
2. Configure snort for IDS mode
     - sudo nano /etc/snort/snort.conf
     - ipvar HOME_NET 192.168.56.0/24
     - ipvar EXTERNAL_NET any
     - sudo snort -T -c /etc/snort/snort.conf
         - we will see a massage of 'Snort successfully validated the configuration'
3. Strart snort in IDS mode
     - sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
         - -A console is used to show alert on the screen
         - -i eth0 is specifying lisetning a traffic running on the interface
4. Generate at attack using kali
   - nmap -sS 192.168.56.6
   - nmap -O 192.168.56.6
Based on the nmap scan, alert report were produced on the screen informing the type of scann was run on the system including the ttype of scan like
SYN scann the massage or alert

