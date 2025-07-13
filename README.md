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

<img width="1919" height="1020" alt="question 1" src="https://github.com/user-attachments/assets/b331fd54-77c0-4571-b35f-ef2ca268b0b8" />

Answer: 172.16.165.165
Step: Apply filter http.request and observe consistent request source IP. This IP appears repeatedly in the capture making outbound HTTP requests to various domains. The high frequency and diversity of domains contacted by this IP indicate suspicious automated behavior.

2) What is the host name of the Windows VM that gets infected?

<img width="1919" height="1019" alt="question 2" src="https://github.com/user-attachments/assets/42612ca9-d877-4400-b348-69809e1ae43b" />

Answer: K34EN6W3N-PC<00>
Step: Apply filter nbns and inspect Name Query Request packet where source is 172.16.165.165. The hostname is retrieved from NBNS traffic where the infected system broadcasts its NetBIOS name, which is common in Windows environments.

3) What is the MAC address of the infected VM?
Answer: f0:19:af:02:9b:f1
Step: Inspect Ethernet II header for packets sourced from 172.16.165.165. This MAC consistently appears as the source in relevant traffic, confirming it as the infected host’s hardware address.

4) What is the IP address of the compromised web site?
Answer: 82.150.140.30
Step: Find HTTP traffic from the infected host to www.ciniholland.nl. DNS resolution or IP header inspection shows this domain maps to 82.150.140.30. There is a large volume of unusual traffic from the infected host to this IP, including requests for JavaScript and image files that trigger redirects.

5) What is the domain name of the compromised web site?
Answer: www.ciniholland.nl
Step: The infected host accessed several files from this domain. A particular .jpg file caused the host to redirect to another domain that eventually served an exploit. This behavior strongly suggests the image was a decoy and the site was compromised to host malicious scripts or redirects.

6) What is the IP address that delivered the exploit kit and malware?
Answer: 37.200.69.143
Step: Follow the redirect chain initiated by traffic from ciniholland.nl. The final domain contacted was stand.trustandprobaterealty.com, which resolves to this IP. The consistent appearance of this IP in requests to index.php?req=... indicates it hosted the actual malware payload.

7) What is the domain name that delivered the exploit kit and malware?
Answer: stand.trustandprobaterealty.com
Step: This domain appears after the infected machine receives a redirect from the compromised site. The format of the URL (index.php?req=...) and the session IDs indicate that this is the malicious server used for the delivery of the exploit kit.
