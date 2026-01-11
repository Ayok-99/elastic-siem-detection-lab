# Elastic SIEM Detection Engineering Lab

## Overview
This project documents a hands-on detection engineering lab built using **Elastic Security SIEM**.  
The goal was to simulate adversary behavior on a Windows endpoint and design detection rules to identify suspicious PowerShell activity.

The lab focuses on:
- PowerShell process execution
- Encoded PowerShell commands
- Detection rule creation and troubleshooting
- Understanding why alerts may fail to fire

---

## Lab Environment
- **SIEM Platform:** Elastic Security
- **Endpoint:** Windows 10 Virtual Machine (VirtualBox)
- **Data Sources:** Elastic Agent / Endpoint Security
- **Attack Simulation:** PowerShell execution via `cmd.exe`

---

## Threat Simulation
The following PowerShell behavior was simulated on the Windows VM:

- PowerShell launched from `cmd.exe`
- Use of the `-EncodedCommand` flag
- Execution of benign payloads (e.g., calculator) to simulate attacker tradecraft

Example command used:
```powershell
powershell.exe -EncodedCommand <Base64EncodedPayload>
```

---

## Lab Walkthrough & Evidence

This section documents the key stages of the detection engineering lab, including environment setup, rule creation, and troubleshooting.

### 1. Elastic Agent Installation (Windows Endpoint)

Elastic Agent was manually installed and enrolled on a Windows 10 virtual machine to collect endpoint telemetry.

![Elastic Agent Installation](screenshots/01_elastic_agent_install_windows.png)

---

### 2. Fleet Agent Health Confirmation

The enrolled agents are visible in Fleet and reporting successfully, confirming data ingestion into Elastic Security.

![Fleet Agents Healthy](screenshots/02_fleet_agents_healthy.png)

---

### 3. Elastic Defend Endpoints Visibility

Elastic Defend is enabled, and endpoints are visible within the Security → Endpoints view.

![Elastic Defend Endpoints](screenshots/03_elastic_defend_endpoints.png)

---

### 4. PowerShell Detection Rule Creation

A custom SIEM detection rule was created to identify PowerShell execution activity on endpoints.

![Detection Rule Created](screenshots/04_powershell_detection_rule_created.png)

---

### 5. Detection Rule Enabled

The custom PowerShell detection rule is enabled and visible alongside other SIEM rules.

![Detection Rule Enabled](screenshots/05_detection_rule_enabled.png)

---

### 6. KQL-Based Troubleshooting in Discover

KQL was used in Discover to search for encoded PowerShell commands.  
Although no results were returned, this step demonstrates investigation and troubleshooting of why alerts may not fire due to telemetry availability or field mappings.

![KQL Troubleshooting](screenshots/06_kql_troubleshooting_no_results.png)

---

## What I Learned

This lab strengthened my understanding of how SIEM detections work end-to-end, from endpoint telemetry ingestion to alert generation and investigation.

Key learnings include:
- How Elastic Agent and Elastic Defend collect and forward endpoint telemetry
- How SIEM detection rules rely on correct data sources and field mappings
- Writing KQL queries to search for suspicious PowerShell activity
- The importance of validating detections in Discover before expecting alerts
- Understanding why alerts may fail to fire even when malicious activity is executed
- Gaining hands-on experience troubleshooting detection logic and data visibility

---

## Why the Alert Did Not Fire

Although the PowerShell command executed successfully on the Windows endpoint, the detection rule did not generate alerts.

This was due to one or more of the following factors:
- The expected fields (such as `process.command_line`) were not populated in the ingested telemetry
- PowerShell execution was captured under a different event schema or dataset than anticipated
- Timing and rule scheduling windows did not align with the executed activity
- Differences between Endpoint Security events and traditional Windows process events

This highlights an important detection engineering concept:  
**A detection rule is only as effective as the telemetry it relies on.**

The investigation demonstrated the need to:
- Validate available fields in Discover before finalizing rule logic
- Adjust detection logic based on actual event schemas
- Avoid assuming alerts will fire without confirming data visibility

---

## Key Skills Demonstrated

- SIEM detection engineering (Elastic Security)
- Endpoint telemetry ingestion and validation
- KQL querying and investigation
- Detection rule creation and tuning
- MITRE ATT&CK alignment (Execution – TA0002)
- Troubleshooting alerting and data gaps
- Security lab documentation and reporting

---

## Disclaimer

This project was conducted in a controlled lab environment for educational purposes only.  
No malicious activity was performed outside of a test virtual machine.
