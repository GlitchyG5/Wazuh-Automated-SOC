# 🛡️ Wazuh Automated SOC – SSH Brute Force Detection & Active Response

## Overview

This project demonstrates an automated Security Operations Center (SOC) workflow built in a VirtualBox homelab using Wazuh.

The lab simulates an SSH brute-force attack from a Kali Linux machine against a monitored Ubuntu endpoint. Wazuh detects the repeated authentication failures and automatically responds by blocking the attacking IP address using its firewall-drop Active Response capability.

After the configured timeout expires, Wazuh automatically removes the firewall block.

## Lab Architecture

| System | Purpose |
|---|---|
| Kali Linux | Simulated attacker |
| Ubuntu Linux | Monitored endpoint |
| Wazuh | SIEM/XDR monitoring, detection, and Active Response |
| VirtualBox | Virtualized lab environment |

### Attack Flow

Kali Linux  
↓  
SSH brute-force attempts  
↓  
Ubuntu endpoint  
↓  
Wazuh detects repeated authentication failures  
↓  
Rule 5712 – SSH brute-force detection  
↓  
Active Response executes firewall-drop  
↓  
Rule 651 – Attacker IP blocked  
↓  
Configured timeout expires  
↓  
Rule 652 – Attacker IP automatically unblocked

## Detection

Multiple SSH authentication attempts were generated using a nonexistent account.

Example:

`fakeuser`

Wazuh identified the repeated authentication failures as an SSH brute-force attack.

**Detection Rule:** `5712`

**Description:** `sshd: brute force trying to get access to the system. Non existent user.`

## Automated Active Response

After detecting the brute-force activity, Wazuh automatically executed:

`active-response/bin/firewall-drop`

The source IP responsible for the attack was temporarily added to the firewall block list.

### Active Response Events

| Rule ID | Event |
|---|---|
| 5712 | SSH brute-force attack detected |
| 651 | Host blocked by firewall-drop Active Response |
| 652 | Host unblocked by firewall-drop Active Response |

This demonstrates an automated:

**Detect → Respond → Block → Recover**

security workflow without requiring manual analyst intervention.

## MITRE ATT&CK

The SSH attack activity maps to:

**T1110 – Brute Force**

Tactic:

**Credential Access**

## Skills Demonstrated

- SIEM monitoring
- Wazuh deployment and configuration
- Linux security monitoring
- SSH attack detection
- Log analysis
- Active Response
- Automated incident response
- Firewall-based containment
- Threat hunting
- MITRE ATT&CK mapping
- VirtualBox homelab administration

## Disclaimer

This project was performed in an isolated cybersecurity homelab for educational purposes. All attack traffic was generated against systems that I own and control.