<img width="1358" height="685" alt="Screenshot 2026-06-13 164120" src="https://github.com/user-attachments/assets/09cd9e2f-9c35-4716-ad42-703467195ecc" />
# 🔥Splunk Real-Time Port Scan Detection (SOC Lab Project)
## 📌Overview

This project demonstrates a real-time intrusion detection system using Splunk SIEM to identify port scanning activity (Nmap reconnaissance) in a simulated network environment.

The system monitors Windows Firewall logs and triggers alerts when a suspicious number of ports are scanned from a single source IP.

## 🎯Objective
```
.Detect network reconnaissance activity
.Identify port scanning behavior (Nmap)
.Generate real-time alerts in Splunk
.Simulate SOC (Security Operations Center) detection workflow
```
## 🧪Lab Environment
```
Component	Details
SIEM Tool	Splunk Enterprise
Target Machine	Windows (192.168.1.70)
Attacker Machine	Kali Linux (192.168.1.71)
Log Source	Windows Firewall Logs (pfirewall.log)
```
## ⚔️Attack Simulation
```
A port scan was performed using Nmap from Kali Linux:

nmap -Pn -p 1-1000 192.168.1.70

This generates multiple connection attempts across different ports, simulating reconnaissance activity.
```
## 🔍Detection Logic (SPL Query)
```
index=firewall src_ip!="127.0.0.1"
| bucket _time span=1m
| stats dc(dest_port) as unique_ports 
        values(dest_port) as scanned_ports 
        count as attempts 
        by src_ip dest_ip _time
| where unique_ports >= 5
```
## 🚨Alert Configuration
```
Alert Type: Real-time
Trigger Condition: Number of Results > 0 (in 5 minutes)
Suppression Rule: src_ip + dest_ip (60 seconds)
Severity: Medium
Action: Email notification + Triggered Alerts
```
## 📧Sample Alert Output
```
When triggered, the alert includes:

Source IP: 192.168.1.71 (Attacker)
Destination IP: 192.168.1.70 (Target)
Number of ports scanned: 5+
Attempts: Multiple connection requests
Detection type: Port Scan / Reconnaissance
```
## 🧠MITRE ATT&CK Mapping
```
Technique	ID	Description
Network Service Discovery	T1046	Port scanning used for reconnaissance
```
## 📸Screenshots
<img width="1358" height="685" alt="Screenshot 2026-06-13 164120" src="https://github.com/user-attachments/assets/f9bff785-bf87-4aef-85c6-4b890b01865e" />
<img width="1363" height="682" alt="Screenshot 2026-06-13 162251" src="https://github.com/user-attachments/assets/b58777b0-f8b1-4581-af17-545c4879d59f" />


<img width="1361" height="689" alt="Screenshot 2026-06-13 160830" src="https://github.com/user-attachments/assets/ec56d773-57b5-4ad3-ac6f-2f554b56b93b" />
<img width="1278" height="752" alt="Screenshot 2026-06-13 142556" src="https://github.com/user-attachments/assets/495e9968-f393-418c-a2d4-7b5571eef299" />

## 📊Key Features
```
✔ Real-time port scan detection
✔ Windows firewall log monitoring
✔ Nmap attack simulation
✔ Automated email alerting
✔ Alert deduplication (throttling)
✔ SOC-style detection logic
```
## 🚀Future Improvements
```
Splunk dashboard for attacker visualization
Multi-stage attack detection (scan → brute force → exploit)
Integration with Splunk Enterprise Security (ES)
Slack / Discord alert integration
Automated incident response workflow
```
## 👨‍💻Author
```
Suraj Magar
Cybersecurity / SOC Lab Project
Focus: SIEM, Threat Detection, Blue Teaming
```
## 📌Note

This project is built for educational SOC simulation purposes using controlled lab environments.
