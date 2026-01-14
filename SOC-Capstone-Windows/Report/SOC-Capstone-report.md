# SOC Capstone Project – Windows Log Analysis & SIEM Detection

## 1. Executive Summary
This project demonstrates hands-on SOC analyst skills by analyzing Windows host logs and correlating security events using Splunk SIEM. Multiple suspicious activities were detected, analyzed, and documented to simulate real-world SOC workflows.

---

## 2. Environment Overview
- Windows 10 Virtual Machine
- Windows Event Logs (Security, PowerShell, Firewall)
- Audit Policies Enabled

---

## 3. Data Sources
| Log Source | Event IDs |
|----------|----------|
| Windows Security Logs | 4624, 4625, 4688, 4698, 4699 |
| PowerShell Logs | 4104 |
| Firewall Logs | 5156 |

---

## 4. Detection Scenarios & Analysis

### 4.1 Failed Logon Activity (Brute Force Indicator)
**Event ID:** 4625  

**Description:**  
- Multiple failed logon attempts were recorded for the same account, followed by a successful authentication.

**Analysis**
- Repeated failures followed by success is a classic brute-force or credential guessing pattern
- Logon Type 2 indicates local interactive access
- Source address was the local host, suggesting hands-on access or local compromise

**Severity**
🟡 Medium (requires correlation)

### 4.2 Suspicious Process Execution – Initial Post-Access Activity
**Event ID:** 4688

**Description**
- PowerShell was launched from Explorer and later from Command Prompt.

**Analysis**
- explorer.exe → powershell.exe is unusual for normal users
- Indicates attacker or advanced user execution
- Command-line context showed enumeration activity

**Severity**
🟠 High

### 4.3 PowerShell Abuse – Encoded Command Execution
**Event ID:** 4104

**Description**
PowerShell Script Block Logging revealed encoded commands.

**Analysis**
- Encoded commands are commonly used to:
- Obfuscate malicious intent
- Evade detection
- Strong indicator of malicious tradecraft

**MITRE ATT&CK**
- T1059.001 – PowerShell

**Severity**
🔴 High

### 4.4 Network Activity – Outbound Connections
**Event ID:** 5156

**Description**
- Outbound connections were initiated by suspicious processes.

**Analysis**
- Identified outbound traffic initiated by PowerShell
- Indicates potential:
- Command-and-control
- External enumeration
- Data exfiltration staging

**Severity**
🟠 High

### 4.5 Persistence via Scheduled Tasks
**Event ID:** 4698 / 4699

**Description**
- A scheduled task was created using administrative privileges to execute PowerShell commands.

**Analysis**
Task included:
-nop
-w hidden
-c
-- Triggered on logon events
-- Strong persistence indicator

**MITRE ATT&CK**
T1053.005 – Scheduled Task

**Severity**
🔴 High

### 4.6 Registry-Based Persistence
**Event ID:** 4657

**Description**
- Registry run key was modified to execute PowerShell at user logon.

**Analysis**
- Registry Run keys are commonly abused for persistence
- Confirms multiple persistence mechanisms, not accidental activity

**MITRE ATT&CK**
T1547.007 – Registry Run Keys

**Severity**
🔴 Critical

## 5. Correlation & Timeline (IMPORTANT)
**Phase	Evidence**
- Initial Access	Failed logons → successful authentication
- Execution	Explorer → PowerShell → CMD
- Defense Evasion	Encoded PowerShell
- Network Activity	Outbound connections
- Persistence	Scheduled Tasks + Registry Run Keys
This is no longer isolated behavior — this is an attack chain.

## 6. Analyst Verdict
🔴 Confirmed Suspicious Activity – Escalation Required
The combination of authentication abuse, suspicious execution, PowerShell obfuscation, outbound connections, and persistence mechanisms strongly indicates system compromise.

## 7. Recommended Response Actions
- Reset affected user credentials
- Review additional endpoints for lateral movement
- Disable malicious scheduled tasks and registry keys
- Isolate host if activity continues
- Perform full malware scan
