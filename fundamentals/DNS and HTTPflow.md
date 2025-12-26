# Lab 01 – DNS Flow Packet Analysis

## Objective
Analyze DNS traffic to understand query–response flow and identify suspicious indicators.

## Environment
- Client: Windows 10
- Network Tool: Wireshark
- Protocol: DNS (UDP/53)

## Display Filters Used
dns
udp.port == 53

## Traffic Flow Observed
1. Client sends DNS query for domain name
2. DNS server responds with resolved IP address
3. Transaction ID matches between request and response

## Key Observations
- Standard A record DNS queries observed
- Query and response timing within normal range
- No excessive or repeated failed queries

## Example Packet Details
- Query Type: A
- Queried Domain: example.com
- Response IP: 93.184.216.34

## Security Relevance
DNS traffic is commonly abused for:
- Command-and-Control (C2)
- DNS tunneling
- Malware beaconing

SOC analysts must monitor for abnormal query frequency or suspicious domains.

## MITRE ATT&CK Mapping
- TA0011 – Command and Control
- T1071.004 – DNS

## SOC Analyst Action
- Monitor repeated DNS queries
- Validate domain reputation
- Escalate if beaconing behavior is detected

## Screenshot
![DNS Query and Response](screenshots/dns-query-response.png)

---
# Lab 02 – HTTP Flow Packet Analysis

## Objective
Analyze HTTP traffic to identify unencrypted data transmission and security risks.

## Environment
- Client: Windows 10
- Server: Test Web Server
- Tool: Wireshark
- Protocol: HTTP (TCP/80)

## Display Filters Used
http
tcp.port == 80

## Traffic Flow Observed
1. Client sends HTTP GET request
2. Server responds with HTTP 200 OK
3. Data transmitted in cleartext

## Key Observations
- URLs and parameters visible
- User-Agent string exposed
- No encryption present

## Example Packet Details
- Method: GET
- Host: testsite.local
- Response Code: 200 OK

## Security Impact
HTTP traffic exposes:
- Session information
- URLs and parameters
- Potential credentials

This makes traffic vulnerable to interception and manipulation.

## MITRE ATT&CK Mapping
- TA0006 – Credential Access
- T1040 – Network Sniffing

## SOC Analyst Response
- Recommend HTTPS enforcement
- Monitor for sensitive data exposure
- Alert if credentials appear in cleartext

## Screenshot
![HTTP GET and Response](screenshots/http-get-response.png)
