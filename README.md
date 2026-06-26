# SIEM Home Lab — Elastic Stack Threat Detection
A home lab simulating a real-world Security Operations Center (SOC) environment using the Elastic Stack (ELK). Built to demonstrate blue team skills including log ingestion, attack simulation, and automated threat detection.

## Project Overview
This project deploys a functional SIEM on a virtualized network, simulates an SSH brute force attack, and detects it using a custom detection rule — mirroring workflows used in real SOC environments.

**Tools & Technologies**
- Elasticsearch 8.19 — log storage and search engine
- Kibana 8.19 — visualization and detection interface
- Filebeat 8.19 — log shipping agent
- Ubuntu Server 22.04 — SIEM host
- Kali Linux 2026.1 — attacker machine
- VirtualBox — virtualization platform
- Hydra — SSH brute force tool

## Architecture
```
+---------------------+         +---------------------------+
|   Kali Linux VM     |         |    Ubuntu Server VM       |
|   192.168.56.1      |         |    192.168.56.101         |
|                     |         |                           |
|   Hydra (SSH BF)    +-------->|   SSH (port 22)           |
|                     |         |   Filebeat                |
+---------------------+         |   Elasticsearch (:9200)   |
                                |   Kibana (:5601)          |
         Host-Only Network      +---------------------------+
         192.168.56.0/24
```

## What I Built

### 1. SIEM Infrastructure
- Deployed Elasticsearch and Kibana on Ubuntu Server VM
- Configured Filebeat with the system module to collect auth logs
- Set up host-only network for isolated attack simulation

### 2. Attack Simulation
- Used Hydra from Kali Linux to perform SSH brute force against the SIEM server
- Generated hundreds of failed authentication attempts
- Verified attack logs appearing in real time via `/var/log/auth.log`

### 3. Threat Detection
- Created a custom Kibana detection rule (Threshold type)
- Rule query: `event.dataset:"system.auth" and system.auth.ssh.event:"Failed"`
- Threshold: 5 or more failures triggers a High severity alert
- Successfully triggered and validated the alert in Kibana Security

## Screenshots

### Elasticsearch Running
![Elasticsearch Running](screenshots/elasticsearch-running.png)

### Kibana Dashboard
![Kibana Dashboard](screenshots/kibana-dashboard.png)

### Logs Flowing into Kibana
![Logs Flowing](screenshots/logs-flowing-kibana.png)

### Brute Force Attack Logs
![Brute Force Logs](screenshots/brute-force-detected-kibana.png)

### Auth Log During Attack
![Auth Log](screenshots/auth-log-brute-force.png)

### Detection Rule Created
![Detection Rule](screenshots/detection-rule-created.png)

### Alert Triggered
![Alert Triggered](screenshots/alert-triggered.png)

## Detection Rule Details

| Field | Value |
|---|---|
| Rule Name | SSH Brute Force Detection |
| Rule Type | Threshold |
| Query | `event.dataset:"system.auth" and system.auth.ssh.event:"Failed"` |
| Threshold | >= 5 events |
| Severity | High |
| Risk Score | 73 |
| Schedule | Every 5 minutes |

## Key Skills Demonstrated
- SIEM deployment and configuration
- Log ingestion pipeline (Filebeat → Elasticsearch)
- Attack simulation in an isolated lab environment
- Custom detection rule creation in Kibana
- Linux server administration
- Network configuration (VirtualBox host-only adapter)
- Blue team / SOC analyst workflows

## Author
Roberto — Economic Informatics Student, ASE Bucharest  
CompTIA Security+ Certified  
GitHub: [Roberto2500](https://github.com/Roberto2500)
