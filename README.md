# 🔥Splunk Real-Time Port Scan Detection (SOC Lab Project)
## 📌Overview

This project demonstrates a real-time intrusion detection system using Splunk SIEM to identify port scanning activity (Nmap reconnaissance) in a simulated network environment.

The system monitors Windows Firewall logs and triggers alerts when a suspicious number of ports are scanned from a single source IP.

## 🎯Objective
.Detect network reconnaissance activity
.Identify port scanning behavior (Nmap)
.Generate real-time alerts in Splunk
.Simulate SOC (Security Operations Center) detection workflow
🧪 Lab Environment
Component	Details
SIEM Tool	Splunk Enterprise
Target Machine	Windows (192.168.1.70)
Attacker Machine	Kali Linux (192.168.1.71)
Log Source	Windows Firewall Logs (pfirewall.log)
⚔️ Attack Simulation

A port scan was performed using Nmap from Kali Linux:

nmap -Pn -p 1-1000 192.168.1.70

This generates multiple connection attempts across different ports, simulating reconnaissance activity.

🔍 Detection Logic (SPL Query)
index=firewall src_ip!="127.0.0.1"
| bucket _time span=1m
| stats dc(dest_port) as unique_ports 
        values(dest_port) as scanned_ports 
        count as attempts 
        by src_ip dest_ip _time
| where unique_ports >= 5
🚨 Alert Configuration
Alert Type: Real-time
Trigger Condition: Number of Results > 0 (in 5 minutes)
Suppression Rule: src_ip + dest_ip (60 seconds)
Severity: Medium
Action: Email notification + Triggered Alerts
📧 Sample Alert Output

When triggered, the alert includes:

Source IP: 192.168.1.71 (Attacker)
Destination IP: 192.168.1.70 (Target)
Number of ports scanned: 5+
Attempts: Multiple connection requests
Detection type: Port Scan / Reconnaissance
🧠 MITRE ATT&CK Mapping
Technique	ID	Description
Network Service Discovery	T1046	Port scanning used for reconnaissance
📸 Screenshots

Add your Splunk screenshots here:

Alert configuration
Triggered alert
Search results dashboard
Nmap scan execution
📊 Key Features
✔ Real-time port scan detection
✔ Windows firewall log monitoring
✔ Nmap attack simulation
✔ Automated email alerting
✔ Alert deduplication (throttling)
✔ SOC-style detection logic
🔧 SPL Enhancements (Optional Improvements)

You can improve detection with:

Severity scoring based on number of ports scanned
Time-based anomaly detection
Multiple attacker correlation
GeoIP enrichment (if external IPs used)
🚀 Future Improvements
Splunk dashboard for attacker visualization
Multi-stage attack detection (scan → brute force → exploit)
Integration with Splunk Enterprise Security (ES)
Slack / Discord alert integration
Automated incident response workflow
👨‍💻 Author

Suraj Magar
Cybersecurity / SOC Lab Project
Focus: SIEM, Threat Detection, Blue Teaming

📌 Note

This project is built for educational SOC simulation purposes using controlled lab environments.
