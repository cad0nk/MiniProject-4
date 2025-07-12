# Malware Traffic Analysis of Exploit Kit Infection from a Compromised Website (malware-traffic-analysis.net)

<img width="1895" height="819" alt="image" src="https://github.com/user-attachments/assets/257fe255-745a-42a7-888d-8dde818a3020" />


## Objective

The objective of this project is to perform network traffic analysis on a PCAP file containing simulated malware activity. The goal is to identify key indicators of compromise (IOC), such as the infected machine's IP and hostname, the compromised website, and the domain that delivered the exploit kit and malware. This analysis simulates a real-world scenario where a Security Operations Center (SOC) analyst must trace malware behavior within a network capture.

### Skills Learned

- Network traffic inspection and filtering using Wireshark.
- Identification of infected hosts through HTTP and NBNS traffic.
- MAC address correlation via Ethernet II analysis.
- Threat hunting methodology: identifying compromised websites and exploit servers.
- Understanding and analyzing malware delivery chains.

### Tools Used

- Wireshark – Used for in-depth packet analysis and protocol inspection.
- PCAP file – Sample provided for threat hunting simulation.

### Level 1 Questions & Analysis

1) What is the IP address of the Windows VM that gets infected?
Answer: 172.16.165.165
Step: Apply filter http.request and observe consistent request source IP.

2) What is the host name of the Windows VM that gets infected?
Answer: K34EN6W3N-PC<00>
Step: Apply filter nbns and inspect Name Query Request packet where source is 172.16.165.165.

3) What is the MAC address of the infected VM?
Answer: f0:19:af:02:9b:f1
Step: Inspect Ethernet II header for packets sourced from 172.16.165.165.

4) What is the IP address of the compromised web site?
Answer: 82.150.140.30
Step: Find HTTP traffic to www.ciniholland.nl, resolve IP from packet.

5) What is the domain name of the compromised web site?
Answer: www.ciniholland.nl
Step: Analyze traffic from infected VM accessing resources (e.g., .jpg/.js) that triggered redirects.

6) What is the IP address that delivered the exploit kit and malware?
Answer: 37.200.69.143
Step: Review connections following redirect, focus on index.php?req=... from stand.trustandprobaterealty.com.

7) What is the domain name that delivered the exploit kit and malware?
Answer: stand.trustandprobaterealty.com
Step: Trace full infection chain and confirm domain from HTTP requests and DNS resolution.
