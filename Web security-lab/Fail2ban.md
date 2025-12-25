### improving login security using Fail2ban to avoid brut force attack
Fail2ban is an open-source intrusion prevention tool for Linux that scans server log files for repeated failed login attempts or malicious patterns and automatically bans the offending IP addresses using firewall rules
steps used to restrict login rules to aviod brut force attack

1. SSH in to the apache server (look the first step above)
   
2. Installation Fail2ban
 
   <img width="1088" height="619" alt="Picture6install" src="https://github.com/user-attachments/assets/89d53359-b7fb-4bed-9397-646231f8fb86" />
   
3. check the status of Fail2ban (sudo systmctl status Fail2ban if it is down then sudo systemctl restart Fail2ban)
   
   <img width="392" height="226" alt="Picture7statustandstart" src="https://github.com/user-attachments/assets/408c0b65-c699-4922-ba08-143c7a025601" />
   
4.  Move to the configuration file ad check the login rules(cd /etc/apache2 and sudo nano jail.conf parameters bantime = 3 minutes, findtime = 10m and maxretry = 3)
   
   <img width="362" height="223" alt="Picture9defualt" src="https://github.com/user-attachments/assets/ab469606-3280-4868-a58d-6c05c053b748" />
   
5. To improve security  i increase the bantime to 10m
  
   <img width="400" height="264" alt="Picture10change" src="https://github.com/user-attachments/assets/aa4eb639-2f0e-4832-b838-4a4913955579" />
6. Checking was run to see how the system behave for failed login
   
   <img width="1279" height="703" alt="Picture11check" src="https://github.com/user-attachments/assets/0e15451d-33c2-4132-93c4-2a766fc357b0" />

