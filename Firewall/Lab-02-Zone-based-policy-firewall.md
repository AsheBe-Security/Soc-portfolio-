## Zone Based Policy Firewall

![0110(1)](https://github.com/user-attachments/assets/2d004981-a227-4cbe-b5b2-722817a67ba5)

Based on the topology i designed using cisco packet tracer, it has three zone, specialist zone with the highest level of security, operation zone where relatively less secured and the Data center protected using DMZ
Based on the gif above
communication between pcs and server were tested and the network is behaving they way designed. little effor were needed to improve address resolution since it lives on the separate server.

**Pinging** from device in one network to the next to show live connectivity

<img width="875" height="875" alt="Screenshot 2026-01-10 215201" src="https://github.com/user-attachments/assets/ee60419d-47d5-440f-934b-cbbdef0ff94d" />

<img width="981" height="966" alt="Screenshot 2026-01-10 215013" src="https://github.com/user-attachments/assets/78363168-4e4b-484d-aa46-c329393e2be9" />

Test both email and web services by involving DNS server for name to ip resolution
For the sake of efficiency, this test were run devices across the network 

<img width="1747" height="856" alt="Screenshot 2026-01-10 214237" src="https://github.com/user-attachments/assets/0c18c8c8-3b6a-4f8e-b2fd-a68adcac6dd5" />

<img width="1742" height="847" alt="Screenshot 2026-01-10 214034" src="https://github.com/user-attachments/assets/a01a483e-e5b1-4c1a-9a08-232f4eb155cf" />

<img width="1738" height="847" alt="Screenshot 2026-01-10 213829" src="https://github.com/user-attachments/assets/96252c47-ae91-44af-84bd-831811bdd6cb" />

---
**steps creating Zone Based Security Policy**
1. **Determining the Zones**
   Based on the topology, the ZPF will only applied on the middle router. Specialist unit will be taken as internal zone as communication from this zone and its returning traffic will be inspected/filtered and any indepedent communication from the outside will be rejected. OPERATION center considered to be less secure and only established connection will be allowed to enter to the SPECIALIST unit. The data center will be considered DMZ (demiliterized Zone)
   
   - R2#config t
   - R2(config)# zone security SPECIALIST
   - R2(config-sec-zone)#exit 
   - R2(config-if)#zone-member security SPECIALIST
   - R2(config-sec-zone)#exit 
   - R1(config)#zone security OPERATION
   - R1(config-sec-zone)#exit
   - R2(config-sec-zone)#zone-member security OPERATION
   - R2(config-sec-zone)#exit 
   - R1(config)#zone security DMZ
   - R1(config-sec-zone)#exit  
   - R1(config)#
   
2. **Specify the traffic**
   For a trrafic going from the private to OPERATION, we are going to apply ACL
   
   - R2(config)#ip access-list 100 permit ip any any
     
   As soon aswe put zones on the topology, all form of communication will be blocked, so inorder to to achive the communication from the SPECIALIST zone, i created ACL which will allows any traffic to and from.

   - R2(config)#ip access-list 101 permit ip any host 192.168.12.4
   - R2(config)#ip access-list 101 permit ip any host 192.168.12.2
     
   These two ACL are designed to allow communication from the OPERATION zone in to the DMZ to access specifically the email and web service specified with its ip address.

   - R2(config)# class-map type inspect match-all  **SPECIALIST-ACL-CLASS**
   - R2(config-cmap)#match access-group 100
   - R2(config) policy map type inspect **SPECIALIST-ACL-POLICY**
   - R2(config-pmap)#class type inspect SPECIALIST-ACL-CLASS
   - R2(config-pmap)#inspect
   This policy will inspect any traffic going out of the SPECIALIST zone and allowed only returning traffic.

Determining the traffic to which the policy will be applied was done by using a class-map. Within the class packets are filtered based on the set of criteria. if we want the packet to fulfill atleast one of the criteria, match-any will be applied but if all criteria should be uphold, match-all will be used. For the SPECIALIST zone we will use match-all option for retrict security.
   - R2(config)# class-map type inspect match-any **SPECIALIST-OPERATION-CLASS**
   - R2(config-cmap)#match protocol icmp
   - R2(config-cmap)#match protocol http
   - R2(config-cmap)#match protocol https
   - R2(config-cmap)#match protocol smtp
   - R2(config-cmap)#match protocol imap
   - R2(config-cmap)#match protocol dns
let us assign this class in to my policy     
   These are the traffic which will be allowed to leave from the SPECIALIST zone to the OPERATION zone.
I have also created another class map for communication Private and DMZ

   - R2(config) policy map type inspect **SPECIALIST-OPERATION-POLICY**
   - R2(config-pmap)#class type inspect SPECIALIST-OPERATION-CLASS
   - R2(config-pmap)#inspect
Let us created a policy pair
   - R2(config)#zone-pair security SPECIALIST-2-DMZ source SPECIALIST destination DMZ
   - R2(config-zpair)# service-policy type inspect SPECIALIST-OPERATION-POLICY
          
My last class map is for the traffic travelling between SPECIALIST and DMZ, inorder to apply both the protocols and acl in to our class, separate class was created for the protocolo with match-any and with ACL using match-all option in to my class

- R2(config)# class-map type inspect match-any DNS-HTTP-CLASS
- R2(config-cmap)#match protocol dns
- R2(config-cmap)#match protocol http
- R2(config-cmap)#match protocol icmp
- R2(config-cmap)#class-map type inspect match-any SMTP-CLASS
- R2(config-cmap)#match protocol smtp
- R2(config-cmap)#class-map type inspect match-all DNS-HTTP-ACL-CLASS
- R2(config-cmap)#match access-group 101
- R2(config-cmap)#match class-map DNS-HTTP-CLASS
- R2(config-cmap)#class-map type inspect match-all SMTP-ACL-CLASS
- R2(config-cmap)#match access-group 102
- R2(config-cmap)#match class-map SMTP-CLASS

- R2(config)#policy-map type inspect OPERATION-DMZ-POLICY
- R2(config-pmap)#class type inspect DNS-HTTP-ACL-CLASS
- R2(config-pmap-c)#inspect
- R2(config-pmap)#class type inspect SMTP-ACL-CLASS
- R2(config-pmap-c)#inspect
- R2(config)#zone-pair security OPERATION-DMZ source OPERATION destination DMZ
- R2(config-zpair)#service-policy type inspect 
   
4. **Define the actions**
after the classes are established with the criteria, the next step in to define what action to take for the traffic which meets the criteria. This can be achived with the policy map. under the policy map    three action can be carriedout, **inspect**, **pass** and **drop**

Policy for Zone one - Any traffic going out from the SPECIALIST zone
   - R2(config) policy map type inspect **SPECIALIST-ACL-POLICY**
   - R2(config-pmap)#class type inspect SPECIALIST-ACL-CLASS
   - R2(config-pmap)#inspect

   - R2(config) policy map type inspect **SPECIALIST-OPERATION-POLICY**
   - R2(config-pmap)#class type inspect SPECIALIST-OPERATION-CLASS
   - R2(config-pmap)#inspect
     
From SPECIALIST to DMZ

 - R2(config)#policy-map type inspect OPERATION-DMZ-POLICY
 - R2(config-pmap)#class type inspect DNS-HTTP-ACL-CLASS
 - R2(config-pmap-c)#inspect
 - R2(config-pmap)#class type inspect SMTP-ACL-CLASS
 - R2(config-pmap-c)#inspect
   
  **inspect** - a state based traffic control where the router will keep security or session information while allowing the traffic the go out and return as a request to the zone by denying the rest of communication
  
5. **Identify a Zone-Pair and Match to a Policy**
Two zones were created at the begning, so we need to create zone-pair to apply the matching criteria and implement the action specified in the policy.

    - R1(config)#zone-pair policy SPECIALIST-OPERATION  source SPECIALIST destination OPERATION
    - R1(config-sec-zone-pair)#service-policy type inspect FILTER-INBOUND
   Now the policy within the policy map are assigned in to the zone pair, so that any memeber of this zone, the traffic will be filter based on the policy action

6. **Assing Zones in to the interface**

   - R1(config)# interface FastEthernet0/0
   - R1(config-if)#zone-member security SPECIALIST
   - R1(config-if)# interface Serial0/1/0
   - R1(config-if)#zone-member security OPERATION
   - R1(config-if)# interface GigabiteEthernet0/0
   - R1(config-if)#zone-member security DMZ 
