# Enterprise SIEM Engineering & SOC Automation Lab

An end-to-end Security Operations Center (SOC) home lab simulation modeling active threat behaviors and engineering a centralized Security Information and Event Management (SIEM) data logging pipeline.

## 🏗️ Architectural Topology & Environment Map
*   **Threat Actor Engine:** Kali Linux Virtual Machine
*   **Target Enterprise Node:** Windows 10 Workstation (IP Network Endpoint: 192.168.28.130)
*   **Central SIEM Hub:** Splunk Enterprise (Active listening node configured on receiving port 9997)
*   **Log Ship Transport:** Splunk Universal Forwarder Client

## ⚙️ Log Pipeline Configuration & Engineering Rules
1. **Receiver Ingestion Setup:** Configured background Splunk network listeners over localized destination port `9997` to accept external application event records.
2. **Endpoint Transport Routing:** Modified system operational `inputs.conf` directives to actively monitor and pack local system audit logs directly into the centralized `main` indexing partition.
3. **Data Pipeline Verification:** Evaluated stream counts dynamically inside the search grid to confirm processing health.

## 💥 Simulated Incident Playbooks (Attack Vectors)
### 1. Network Mapping & Port Reconnaissance (Nmap Probe)
*   **Execution Rule:** `sudo nmap -A -v 192.168.28.130`
*   **Telemetry Impact:** Generated high-density connection trails across common service ports as Kali scanned the target boundaries.

### 2. Password Spraying & Brute-Force Simulation (Hydra Attack)
*   **Execution Rule:** `hydra -l Administrator -P passwords.txt 192.168.28.130 smb`
*   **Telemetry Impact:** Triggered high-frequency failed logon alerts, creating distinct credential-stuffing log volumes.

## 📊 Live Splunk SIEM Monitoring Studio Dashboard
Below is the operational production monitoring view built dynamically to map the lab's network footprint:

### Executive Security Analytics Overview
![Executive Dashboard View](Screenshot%20(1).png)

### Security Incident Activity Trends
![Activity Trends Graph](Screenshot%20(2).png)

### Ingestion Metrics & Log Volumetrics
![Log Volumetrics Tracker](Screenshot%20(3).png)



### 📈 Panel Insights & Metrics Tracked:
*   **Total Logs Ingested:** Tracks total event volume hitting our monitoring pipelines in real-time.
*   **Enterprise Security Event Trends (24h):** A multi-colored column graph displaying security activity time spikes.
*   **Authentication Security Overview:** A detailed bar graph isolating login success-to-failure volume ratios.
