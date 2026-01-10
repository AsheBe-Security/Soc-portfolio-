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
   Specialist unit will be taken as internal zone as communication from this zone will be inspected/filtered and any indepedent communication from the outside will be rejected. OPERATION center considered to be less secure and only established connection will be allowed to enter to the SPECIALIST unit. The data center will be considered DMZ (demiliterized Zone)
   
   - R1#config t
   - R1(config)# zone security SPECIALIST
   - R1(config-sec-zone)#exit
   - R1(config)zone security OPERATION
   - R1(config-sec-zone)#exit
   - R1(config)#zone security DMZ
   - R1(config-sec-zone)#exit
   - R1(config)#
   
2. **Specify the traffic**
   determining the traffic to which the policy will be applied was done by using a class-map. Within the class packets are filtered based on the set of criteria. if we want the packet to fulfill atleast one of the criteria, match-any will be applied but if all criteria should be uphold, match-all will be used. For the SPECIALIST zone we will use match-all option for retrict security.
   - R1(config)# class-map type inspect match-any **INBOUND-TRAFFIC**
   - R1(config-cmap)#match protocol icmp
   - R1(config-cmap)#match protocol http
   - R1(config-cmap)#match protocol https
   - R1(config-cmap)#match protocol smtp
   - R1(config-cmap)#match protocol imap
   - R1(config-cmap)#match protocol dns
     
   These are the traffic which will be allowed to leave the zone in to the public
   
4. **Define the actions**
after the classes are established with the criteria, the next step in to define what action to take for the traffic which meets the criteria. This can be achived with the policy map. under the policy map    three action can be carriedout, **inspect**, **pass** and **drop**
   - R1(config)# policy-map type inspect FILTER-INBOUND
   - R1(config-pmap)# class type inspect INBOUND-TRAFFIC
   - R1(config-pmap-c)#inspect
     
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
