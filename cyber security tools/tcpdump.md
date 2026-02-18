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
