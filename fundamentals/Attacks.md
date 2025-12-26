
# Lab 03 – SSH Brute Force Detection

## Objective
Detect SSH brute-force attempts using packet capture analysis.

## Filter Used
tcp.port == 22

## Findings
- Multiple failed connection attempts from a single IP
- Repeated SYN packets within short intervals

## Indicators
- High connection frequency
- No successful authentication packets

## MITRE ATT&CK
- T1110 – Brute Force

## SOC Response
- Block source IP
- Investigate lateral movement
- Escalate if internal host targeted

