# Lab 01 Access Control list

**First step - Design network topology**

![1228(1)](https://github.com/user-attachments/assets/5575b592-b746-419f-b8d7-e61130787905)

IP address 

<img width="862" height="248" alt="Screenshot 2025-12-29 000143" src="https://github.com/user-attachments/assets/dc3df7c6-7d04-4b87-a3f8-7ffdd4305458" />

**Determin the purpose of the Access Control Lists**
- Determin the type of traffic allowed to go outside the internet
- Deny any other unwanted traffic access in to the private network
- Monitor and control cross communication between the pcs within the network

Due to its applicability on the layer 3 and four of the OSI model, i will be using extended ACL for this lab
- R1 enable
- R1# config terminal
- R1(config)# ip access-list extended

INBOUNDS 

- R1(config-ext-nacl)# Remark Permits inside HTTP and HTTPS traffic
- R1(config-ext-nacl)# permit tcp 192.168.1.2 0.0.0.255 any eq 80 (HTTP)
- R1(config-ext-nacl)# permit tcp 192.168.1.2 0.0.0.255 any eq 443 (HTTPS)
- R1(config-ext-nacl)# permit tcp 192.168.1.3 0.0.0.255 any eq 443 (HTTPS)
- R1(config-ext-nacl)# permit icmp 192.168.2.3 0.0.0.255 192.168.1.2 0.0.0.255 (remark only ping is allowed between pc with the networks)
- R1(config-ext-nacl)# exit
- R1(config)#
- R1(config)# ip access-list extended

OUTBOUND

- R1(config-ext-nacl)# Remark Only permit returning HTTP and HTTPS traffic
- R1(config-ext-nacl)# permit tcp any 192.168.1.2 0.0.0.255 192.168.1.3 0.0.0.255   established
- R1(config-ext-nacl)# exit

Implementation of ACL to the interface

- R1(config)# interface g0/0/0
- R1(config-if)# ip access-group SURFING in
- R1(config-if)# ip access-group BROWSING out
- R1(config-if)# end
