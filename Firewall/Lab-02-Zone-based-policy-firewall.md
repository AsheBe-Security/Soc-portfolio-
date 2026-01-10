## Zone Based Policy Firewall

![0110(1)](https://github.com/user-attachments/assets/2d004981-a227-4cbe-b5b2-722817a67ba5)

Based on the topology i designed using cisco packet tracer, it has three zone, specialist zone with the highest level of security, operation zone where relatively less secured and the Data center protected using DMZ
Based on the gif above
communication between pcs and server were tested and the network id behaving they way designed. little effor were needed to improve address resolution since it lives on the separate server.

**steps creating Zone Based Security Policy**
1. Determining the Zones
   Specialist unit will be taken as internal zone as communication from this zone will be inspected/filtered and any indepedent communication from the outside will be rejected.
   
   - R1#config t
   - R1(config)# zone security SPECIALIST
   - R1(config-sec-zone)#exit
   - R1(config)zone security OPERATION
   - R1(config-sec-zone)#exit
   - R1(config)#
   
2. Specify the traffic
   determining the traffic to which the policy will be applied was done by using a class-map. Within the class packets are filtered based on the set of criteria. if we want the packet to fulfill atleast one of the criteria, match-any will be applied but if all criteria should be uphold, match-all will be used. For the SPECIALIST zone we will use match-all option for retrict security.
   - R1(config)# class-map type inspect match-all **INBOUND-TRAFFIC**
   - R1(config-cmap)#match protocol icmp
   - R1(config-cmap)#match protocol http
   - R1(config-cmap)#match protocol https
   - R1(config-cmap)#match protocol smtp
   - R1(config-cmap)#match protocol imap
   - R1(config-cmap)#match protocol dns
     
   These are the traffic which will be allowed to leave the zone in to the public
   
4. Define the actions
after the classes are established with the criteria, the next step in to define what action to take for the traffic which meets the criteria. This can be achived with the policy map. under the policy map    three action can be carriedout, **inspect**, **pass** and **drop**
   - R1(config)# policy-map type inspect FILTER-INBOUND
   - R1(config-pmap)# class type inspect INBOUND-TRAFFIC
   - R1(config-pmap-c)#inspect
     
  **inspect** - a state based traffic control where the router will keep security or session information while allowing the traffic the go out and return as a request to the zone by denying the rest of communication
  
5. Identify a Zone-Pair and Match to a Policy
Two zones were created at the begning, so we need to create zone-pair to apply the matching criteria and implement the action specified in the policy.

    - R1(config)#zone-pair policy SPECIALIST-OPERATION  source SPECIALIST destination OPERATION
    - R1(config-sec-zone-pair)#service-policy type inspect FILTER-INBOUND
   Now the policy within the policy map are assigned in to the zone pair, so that any memeber of this zone, the traffic will be filter based on the policy action
6. Assing Zones in to the interface

   - R1(config)# interface FastEthernet0/0
   - R1(config-if)#zone-member security SPECIALIST
   - R1(config-if)# interface Serial0/1/0
   - R1(config-if)#zone-member security OPERATION
