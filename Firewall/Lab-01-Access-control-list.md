# Lab 01 Access Control list

**1. First step - Design network topology**

![0106(1)](https://github.com/user-attachments/assets/ac1b0575-5308-4012-9144-b89946ba27d9)


IP address 

<img width="940" height="358" alt="Screenshot 2026-01-07 185715" src="https://github.com/user-attachments/assets/2eeebc83-1640-4bfa-b421-2698cfe8dddd" />


**2. Determin the purpose of the Access Control Lists**
- Determin the type of traffic allowed to go outside the internet
- Deny any other unwanted traffic access in to the private network
- Monitor and control cross communication between the pcs within the network

**3. write the commandline using text editor and apply it on the router cli**

Due to its applicability on the layer 3 and four of the OSI model, i will be using extended ACL for this lab

**INBOUND**

- R1 enable
- R1# config terminal
- R1(config)# ip access-list extended INBOUND
- R1(config-ext-nacl)# Remark Permits inside HTTP and HTTPS traffic
- R1(config-ext-nacl)# permit tcp host 192.168.1.2 any eq 80
- R1(config-ext-nacl)# permit tcp host 192.168.1.2 any eq 443
- R1(config-ext-nacl)# permit tcp host 192.168.1.3 any eq 443 
- R1(config-ext-nacl)# permit tcp host 192.168.1.3 any eq 25 
- R1(config-ext-nacl)# permit icmp host 192.168.1.2 host 192.168.2.2 echo 
- R1(config-ext-nacl)# permit icmp host 192.168.1.3 host 192.168.2.3 echo
- R1(config-ext-nacl)# deny ip any any
- R1(config-ext-nacl)# exit
- R1(config)#

  **OUTBOUND**
  
- R1(config)#
- R1(config)# ip access-list extended OUTBOUND
- R1(config-ext-nacl)# Remark Only permit returning HTTP and HTTPS traffic
- R1(config-ext-nacl)# permit tcp any host 192.168.1.2 established
- R1(config-ext-nacl)# permit tcp any host 192.168.1.3 established
- R1(config-ext-nacl)# permit icmp host 192.168.2.2 host 192.168.1.2 echo-reply
- R1(config-ext-nacl)# permit icmp host 192.168.2.3 host 192.168.1.3 echo-reply
- R1(config-ext-nacl)# deny ip any any
- R1(config-ext-nacl)# exit

**4. Implementation of ACL to the interface**

- R1(config)# interface g0/0/0
- R1(config-if)# ip access-group INBOUND in
- R1(config-if)# ip access-group OUTBOUND out
- R1(config-if)# end

---
**5. After Implementation of ACL on to the Routers**
A picture below shows the configuration of aces in the acl on the cisco router 

<img width="807" height="300" alt="Screenshot 2026-01-01 170529" src="https://github.com/user-attachments/assets/a60b9b3a-6081-4bbb-a37a-aee51b62a7c3" />

After creating the ACL, it will be assigned to the interface using command
R1(config)# interface FastEthernet0/0
R1(config-if)# ip access-group INBOUND in
R1(config-if)# ip access-group OUTBOUND out
R1(config-if)# exit

<img width="567" height="383" alt="Screenshot 2026-01-01 173135" src="https://github.com/user-attachments/assets/2d1da851-e891-4ccb-bd8e-24e4ad633eb1" />

R1(config)# write memory (remark for saving the acl configuration in to the router)

<img width="565" height="365" alt="Screenshot 2025-12-31 192706" src="https://github.com/user-attachments/assets/eca7e247-6bf8-45e2-9acb-b0199699d286" />

Network A was allowed to sent out ICMP (ping) packets to the other side and its replay will be accepted. the same will happen to the http/https and email traffic.

<img width="611" height="627" alt="Screenshot 2025-12-31 195029" src="https://github.com/user-attachments/assets/85d09ae3-a1cc-4801-8588-156db0eb6eb0" />
<img width="746" height="557" alt="Screenshot 2025-12-31 192754" src="https://github.com/user-attachments/assets/2b2d7f9e-6f8c-40c2-9b35-34994f247661" />

---

## **Website and Email service access control**

Steps i carried out
1. connect all pcs with the server

   - I stablish a dummy domian name ('mail.com')
   - DNS server was configured within the same server for the email to resolve name to ip address
     
2. Connection was tested by pinging to the server from all pcs using both the domain or server ip
   
    - initially only ping to the ip address was possible, after running network troubleshooting, firewall issue was fixed and ping to the domian was achieved
    - 
 <img width="662" height="518" alt="Screenshot 2026-01-09 184335" src="https://github.com/user-attachments/assets/78914f42-a692-4b88-a579-42ad1ce10eca" />
 
 <img width="758" height="557" alt="Screenshot 2026-01-09 184227" src="https://github.com/user-attachments/assets/329300c9-5c43-4c6c-8b21-77008ef1fcff" />

     
3. testing email was exchanged between pcs to see of dns server was working, all emails was sent aand recieved.
   
<img width="1857" height="847" alt="Screenshot 2026-01-09 184626" src="https://github.com/user-attachments/assets/c440037d-8227-4744-8e70-c8785803fc10" />

