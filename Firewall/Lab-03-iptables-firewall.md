## Lab 03 - Iptables
It is a firewall installed by defualt on alll distribution of UBUNTU. Without any chnage, Iptable allows traffic to flow through the OS. Traffics are filtered based on a certian set of rule which specifies ports and protocols.

The defualt iptables rules viewed by using the command line

   - sudo iptables -L

<img width="865" height="391" alt="Screenshot 2026-01-18 031810" src="https://github.com/user-attachments/assets/61c09263-05b8-4476-a9db-345ccc5e473c" />

Iptables has three chains which governs the traffic rules in accepting and droping a traffic coming in and going out.

### Flush existing rules
- iptables -F
- iptables -X

### Default policies: DROP everything
- iptables -P INPUT DROP
- iptables -P OUTPUT DROP
- iptables -P FORWARD DROP
  
<img width="639" height="350" alt="Screenshot 2026-01-19 025639" src="https://github.com/user-attachments/assets/ed8bea01-cff4-4a12-8d12-0aa8944a8db3" />

### Allow loopback traffic
- iptables -A INPUT -i lo -j ACCEPT
- iptables -A OUTPUT -o lo -j ACCEPT
  
<img width="641" height="237" alt="Screenshot 2026-01-19 030126" src="https://github.com/user-attachments/assets/89b24b66-ce70-43dd-b153-85c0865c0979" />

### Allow established and related connections (RETURN traffic)
- iptables -A INPUT  -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
- iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

## ALLOW OUTGOING + RETURN


### SMTP
- iptables -A OUTPUT -p tcp --dport 25 -m conntrack --ctstate NEW -j ACCEPT

### DNS (TCP & UDP)
- iptables -A OUTPUT -p udp --dport 53 -m conntrack --ctstate NEW -j ACCEPT
- iptables -A OUTPUT -p tcp --dport 53 -m conntrack --ctstate NEW -j ACCEPT

### DHCP (client)
- iptables -A OUTPUT -p udp --sport 68 --dport 67 -j ACCEPT
- iptables -A INPUT  -p udp --sport 67 --dport 68 -j ACCEPT

### HTTP
- iptables -A OUTPUT -p tcp --dport 80 -m conntrack --ctstate NEW -j ACCEPT

### POP3
- iptables -A OUTPUT -p tcp --dport 110 -m conntrack --ctstate NEW -j ACCEPT

### IMAP
- iptables -A OUTPUT -p tcp --dport 143 -m conntrack --ctstate NEW -j ACCEPT

### HTTPS
- iptables -A OUTPUT -p tcp --dport 443 -m conntrack --ctstate NEW -j ACCEPT



<img width="717" height="585" alt="Screenshot 2026-01-19 031825" src="https://github.com/user-attachments/assets/4ef7a7a5-738d-465b-8758-e81d3aeef067" />

### Saving the rules in to the config file

- sudo sh -c 'iptables-save > /etc/iptables/rules.v4'
  
<img width="887" height="556" alt="Screenshot 2026-01-19 032007" src="https://github.com/user-attachments/assets/a90a8760-2749-4019-abc1-42cdd4da2d8b" />



