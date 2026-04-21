# SEIM lab using splunk

**🎯 Lab Objectives**

By the end of this lab, i will be able to:
- Understand how Splunk works as a SIEM
- Ingest log data
- Run searches and create alerts
- Detect a basic security event (failed logins)
Tools
for this lab, required
- Linux as attack machine
- download splunk
- windows/ubuntu server for generating logs
  #### Part One
1.Download ubuntu OS
    - For Splunk server Vbox
    - For Apache server Vbox
2. download splunk and splunk universalforwarder
   - On the splunk server Vbox, open the terminal (avoid using the root ) and copy paste the dowloding link to the splunk enterprise
       - wget -O splunk-10.2.2-80b90d638de6-linux-amd64.tgz "https://download.splunk.com/products/splunk/releases/10.2.2/linux/splunk-10.2.2-80b90d638de6-linux-amd64.tgz" 
   -On the apache server download both splunk universalforwarder and apache2 using the terminal but cal also be downloaded locally 
       - wget -O splunkforwarder-10.2.2-80b90d638de6-linux-amd64.tgz "https://download.splunk.com/products/universalforwarder/releases/10.2.2/linux/splunkforwarder-10.2.2-80b90d638de6-linux-amd64.tgz"
       - sudo apt install apache2
3.On the splunk server 
  -let us create splunk user
     - first switch to root user
           - sudo su
     - useradd -s /bin/bash -d /opt/splunk -m splunk (we have created splunk user lives within the home directory /opt/splunk
     - check if the user exists by moving in to the user
     - su - splunk
     - whoami
     - after moving in to the home direcotry of splunk (/opt/splunk), try to extract the tgz file we downloaded earlier
         - tar -xzvf splunk-8.0.4.1-ab7a85abaa98-Linuxx86_64 -C /opt/
     - sudo /opt/splunk/bin/splunk start --accept-license
     - provide username and password
 4. once the splunk started, we can open the splunk web interface using your splunk ip running on port 8000
         - 192.168.0.80:8000
           
   <img width="1897" height="742" alt="image" src="https://github.com/user-attachments/assets/9c8d68d0-6f77-42ab-965a-cb343486a2f5" />
###Part two 
regarding splunk universalforwarder
1. Open the apache server vbox
2. on the terminal window use the command
      - sudo su (to switch in to the root user)
      - useradd -s /bin/bash -d /opt/splunkforwarder -m splunk (Create new user called splunk with home directory at 
this location /opt/splunkforwarder)
      - su - splunk ( to switch to splunk user)
3. Extract the downloaded tgz file in to the directory of /opt/splunkforwarder
     - tar -xzvf splunkforwarder-8.0.4-767223ac207fLinux-x86_64.tgz -C /opt/
4. start the splunk forwarder
     - /opt/splunkforwarder/bin/splunk start ----accept----
       
    <img width="1065" height="702" alt="image" src="https://github.com/user-attachments/assets/8ed2379a-3139-46e0-9a4f-49ad36693da4" />
####Part three configure splunk to recieve apache logs
1. go to the virtual machine which runs  splunk server and start splunk using terminal
     - sudo su - splunk
     - /opt/splunk/bin/splunk start
     - open your browser and type in your ubuntu ip address with it port number 8000
           - xx.xx.xx.xx:8000
2. Go to splunk web interface, go to setting and under data click forwarding and recieving option
     - click on receiving
     - click new receiving ports
     - type in port number 9997 and save
       
   <img width="1902" height="402" alt="image" src="https://github.com/user-attachments/assets/f75f4d8e-45c7-47b0-b004-bc1078e29ba5" />

3. Create the index where the log data lives in
     - Go to setting
     - click on indexes  
