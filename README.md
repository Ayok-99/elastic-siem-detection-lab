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
- Use of `-EncodedCommand` flag
- Execution of benign payloads (e.g., calculator) to simulate attacker tradecraft

Example command used:
```powershell
powershell.exe -EncodedCommand <Base64EncodedPayload>
