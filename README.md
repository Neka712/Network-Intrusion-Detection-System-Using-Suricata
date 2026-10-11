# Network-Intrusion-Detection-System-Using-Suricata
# CodeAlpha_NetworkIntrusionDetectionSystem

##  Overview
This project is part of the CodeAlpha Cybersecurity Internship (Task 4: Network Intrusion Detection System). It demonstrates the setup of a network-based Intrusion Detection System (IDS) using Suricata, including custom detection rules, live traffic monitoring, and an automated response mechanism for detected intrusions.

##  Objective
- Set up a network-based IDS using Suricata
- Configure custom rules and alerts to detect suspicious or malicious activity
- Monitor network traffic continuously for potential threats
- Implement a response mechanism for detected intrusions

##  Tools & Environment
- OS: Kali Linux (Oracle VirtualBox)
- IDS Engine: Suricata 8.0.7
- Traffic generation: ping (ICMP), nmap (TCP port scan), curl + Python's http.server (HTTP)
- Response enforcement: Bash script + iptables

##  Custom Detection Rules
Defined in /etc/suricata/rules/local.rules:

alert icmp any any -> any any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
alert tcp any any -> any any (flags:S; msg:"TCP SYN Detected"; sid:1000003; rev:1;)
alert http any any -> any any (content:"GET"; msg:"HTTP GET Request Detected"; sid:1000005; rev:1;)

##  Setup Summary
1. Installed Suricata via apt
2. Wrote custom rules in local.rules
3. Corrected suricata.yaml to point rule-files and default-rule-path at the custom rules file (a default misconfiguration otherwise prevents rules from loading)
4. Verified rule loading via the Suricata log (X signatures processed)
5. Generated real test traffic (ping, nmap, curl) and confirmed matching alerts in /var/log/suricata/fast.log

##  Response Mechanism
Since Suricata ran in IDS (passive) mode, a companion script (/usr/local/bin/suricata_response.sh) was built to:
- Tail the alert log in real time
- Count repeated TCP port-scan alerts per source IP
- Automatically block a source IP with iptables after 5 alerts
- Log every block action with a timestamp to /var/log/suricata_response.log

Verified with:
sudo iptables -L INPUT -v -n

##  Full Report
See Suricata_IDS_Report.docx in this repository for the complete writeup, including all configuration steps, screenshots, and findings.

##  Author
Philomena Ekhaizor— CodeAlpha Cybersecurity Internship
