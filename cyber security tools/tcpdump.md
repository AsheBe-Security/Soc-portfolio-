# Lab - Analyzing traffic Packet using tcpdump

### 🎯 Lab Overview

This lab is designed to take you from tcpdump fundamentals to advanced packet analysis, 
mirroring real-world troubleshooting and security investigation scenarios. By the end, 
you’ll be comfortable capturing, filtering, and interpreting network traffic like a junior network engineer or SOC analyst.
Step1: Make sure tcpdump is running on your system
     - tcpdump --version
    if not, install using the command 
      - sudo apt update && sudo apt install tcpdump -y
Step2: Understand your network interface
      - tcpdump -D or
      - ip a
      
<img width="891" height="666" alt="Screenshot 2026-02-18 101929" src="https://github.com/user-attachments/assets/21e6d5a3-ff43-4eab-b713-3da56121fc6c" />

 Step3: launch tcpdump to capture the first packet
       - sudo tcpdump -i eth0
         - with this command, tcpdump will run capturing packet infinitely, so foced stop can be applied using 
           - right control c (it will show you the number of captured packets in one single tcpdump scan)
           
<img width="875" height="687" alt="Screenshot 2026-02-18 102647" src="https://github.com/user-attachments/assets/c195c15b-17ac-4c03-8657-96790b4e1e9c" />

Step4: a limited number of packet can be captured using the count option (-c)
    - sudo tcpdump -c 10 -i eth0 (only ten number of packet can be captured and tcpdump will automatically stop)
    
<img width="907" height="381" alt="Screenshot 2026-02-18 103223" src="https://github.com/user-attachments/assets/a80d2f99-8561-450e-9d5b-9714a580a111" />
    
Step5: tcpdump can be specified to a protocols or ports
      - sudo tcpdump -i eth0 icmp 
         - ping to a host (google.com)(packet being captured without limits)
   <img width="882" height="701" alt="Screenshot 2026-02-18 105006" src="https://github.com/user-attachments/assets/646154c8-2cff-4bbe-8ec2-5f71e9ea49e4" />
   
  <img width="816" height="682" alt="Screenshot 2026-02-18 104814" src="https://github.com/user-attachments/assets/ac3fa24a-01d4-43b8-8355-d10ca3ac03dc" />
 
   - sudo tcpdump -c 10 -i eth0 icmp
      
  <img width="908" height="287" alt="Screenshot 2026-02-18 105113" src="https://github.com/user-attachments/assets/2b52c3fd-3a18-4b92-b8db-85305dbc6e01" />

   - sudo tcpdump -c 10 -i eth0 80
   - sudo tcpdump -c 10 -i eth0 443
     
   <img width="897" height="480" alt="Screenshot 2026-02-18 104200" src="https://github.com/user-attachments/assets/14559ec9-72a3-416b-a03b-0bfd34597761" />
   
<img width="907" height="381" alt="Screenshot 2026-02-18 103223" src="https://github.com/user-attachments/assets/5eee21fb-2bd5-4539-bd71-02cad30d4c7a" />
   
Step6: Scanning can be targeted to specific host (providing ip address)
   - sudo tcpdump -c 10 -i eth0 host 194.168.4.100 (google.com)
