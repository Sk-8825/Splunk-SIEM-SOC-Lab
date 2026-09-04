# Building a SIEM Dashboard in Splunk (SOC Home Lab)

This project details the engineering deployment of an isolated Security Operations Center (SOC) home lab designed to simulate adversarial behaviors and monitor traffic activity using a centralized Splunk SIEM pipeline. 

The infrastructure maps an active attack surface using Kali Linux, routes telemetry from a Windows 10 target workstation endpoint, and applies Splunk Enterprise to dynamically index, query, and visualize incident data on a production-ready dashboard.

## 🏗️ Lab Architecture & Environment Map
*   **Adversarial Engine:** Kali Linux VM (VMware Workspace)
*   **Target Enterprise Endpoint:** Windows 10 Workstation (Host IP: 192.168.28.130)
*   **Centralized SIEM Hub:** Splunk Enterprise (Listening on Data Receiving Port 9997)
*   **Data Transport Engine:** Splunk Universal Forwarder (Installed on Windows 10 Host)

## ⚙️ Data Pipeline Configuration & Logging Rules
1. **Receiver Ingestion Configuration:** Accessing the Splunk web engine control console (`http://localhost:8000`), the Forwarding and Receiving rules were updated to open inbound networking TCP port `9997` to accept external log streams.
2. **Endpoint Source Mapping:** Navigated to `C:\Program Files\SplunkUniversalForwarder\etc\system\local\` on the target endpoint and deployed an active `inputs.conf` configuration profile. Rules were defined to pull operational local Windows Security event channels and forward them to the primary index.
3. **Pipeline Initialization:** Executed a system restart on the `SplunkForwarder` service via Windows Service Manager (`services.msc`) to bind the logging rules and initialize real-time data transport.

## 💥 Simulated Threat playbooks (Adversarial Actions)
### 1. Host Discovery & Boundary Reconnaissance (Nmap Scan)
*   **Execution Command:** `sudo nmap -A -v 192.168.28.130`
*   **Telemetry Impact:** Launched an aggressive host assessment from the Kali Linux terminal to discover open service ports. The operation generated dense connection logs across multiple network sockets on the target endpoint.

### 2. Credential Stuffing & Brute-Force Simulation (Hydra Attack)
*   **Execution Command:** `hydra -l Administrator -P passwords.txt 192.168.28.130 smb`
*   **Telemetry Impact:** Executed a high-frequency authentication dictionary attack targeted at the Windows SMB file-sharing module using a localized text array. This behavior triggered immediate authentication failure spikes across system logs.

## 📊 Live Splunk SIEM Monitoring Studio Dashboard
The following visual panels illustrate the dark-mode Dashboard Studio tracking canvas built to aggregate incoming endpoint logging data during threat generation:

### Executive Security Analytics Overview
![Executive Dashboard View](Screenshot%20(1).png)

### Security Incident Activity Trends
![Activity Trends Graph](Screenshot%20(2).png)

### Ingestion Metrics & Log Volumetrics
![Log Volumetrics Tracker](Screenshot%20(3).png)

### 📈 Panel Insights & Metrics Tracked:
*   **Total Logs Ingested:** A centralized metric counter displaying the cumulative volume of events processed over a 24-hour collection period (over 1,100 operational events captured).
*   **Enterprise Security Event Trends (24h):** A time-series column graph charting data ingestion rates over a sequential timeline, identifying specific processing peaks during attack execution windows.
*   **Authentication Security Overview:** An analytical visualization isolating successful logons against failed system access attempts during brute-force testing.

## 🛡️ Threat Mitigation & Defensive Incident Response Plan
The following defensive controls outline the engineered system updates required to secure the Windows enterprise endpoint from the threat profiles captured on the SIEM canvas:

### 1. Defending Against Network Reconnaissance
*   **Engineering Fix:** Configure advanced firewall connection security rules inside Windows Defender Firewall to drop inbound ICMP echo requests and block scanning attempts originating from untrusted external network segments.
*   **SIEM Monitoring Rule:** Deployed a correlation query alert triggering a high-priority warning if a single unique source IP attempts connections across more than 20 unique network ports within a 10-second threshold.

### 2. Defending Against Authentication Brute-Force Campaigns
*   **Engineering Fix:** Initialize an **Account Lockout Policy** inside the Windows Local Security Policy panel (`secpol.msc`). Setting system thresholds to automatically lock target accounts for 30 minutes after 5 consecutive bad credential attempts terminates automated scanning utilities.
*   **SIEM Monitoring Rule:** Monitor Event ID `4625` (An account failed to log on). Configure real-time query rules to push automated alerts to the security response team if authentication failures for a single username exceed 10 records inside 60 seconds.

## 🗃️ MITRE ATT&CK Matrix Mapping
Active lab exercises are mapped directly to official industry-standard threat tracking taxonomies to classify adversarial behavior:

| Incident Stage | Attack Technique | MITRE ATT&CK ID | SIEM Detection Indicator |
| :--- | :--- | :--- | :--- |
| **Reconnaissance** | Active Scanning | `T1595` | Abnormal volume of distinct port connections from one source node |
| **Credential Access** | Brute Force | `T1110` | High-density frequency spike in Windows Security logs (`EventCode=4625`) |
| **Defense Evasion** | Indicator Removal | `T1070` | Audit rules tracking event log clearing patterns (`EventCode=1102`) |
