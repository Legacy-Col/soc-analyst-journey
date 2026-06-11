# SOC Analyst Home Lab: Operation Payroll Heist

## Overview

Operation Payroll Heist is a hands-on Security Operations Center (SOC) simulation project designed to emulate the lifecycle of a real-world cyber intrusion, from initial access through incident response.

The objective of this project is to develop and demonstrate practical SOC skills by investigating attacker activity within a controlled lab environment. Rather than focusing on offensive exploitation, the project emphasizes detection engineering, log analysis, threat hunting, malware analysis, phishing investigations, incident response, and security automation.

Throughout this simulation, a fictional threat actor targets an employee using a phishing campaign disguised as a payroll-related communication. The resulting attack generates realistic telemetry that is analyzed using industry-standard tools and methodologies.

---

## Project Goals

This project aims to demonstrate the ability to:

* Investigate phishing incidents
* Analyze Windows endpoint telemetry
* Perform malware analysis
* Develop SIEM detections
* Conduct threat hunting activities
* Create YARA detection rules
* Build SOAR workflows
* Produce professional incident reports
* Reconstruct attacker timelines
* Communicate findings to technical and non-technical stakeholders

---

## Lab Environment

### Operating Systems

* Windows 10 (Victim Workstation)
* Kali Linux (Attack Simulation Platform)
* Metasploitable (Internal Target System)

### Security Tools

* Sysmon
* Splunk Enterprise
* Splunk Universal Forwarder
* Tines SOAR
* Thunderbird
* YARA
* FLOSS
* PEStudio
* VirusTotal (IOC Validation)
* AbuseIPDB (IOC Enrichment)

---

## Scenario

A member of the Finance department receives a payroll-related email containing a malicious attachment.

The employee downloads and executes the attachment, resulting in suspicious activity on the workstation.

The SOC must investigate the activity, determine the scope of compromise, identify attacker behavior, develop detections, and produce an incident report.

The project is conducted in phases to simulate a realistic SOC workflow.

---

# Attack Lifecycle

## Phase 1 – Initial Access

A phishing email is delivered to the victim.

Objectives:

* Analyze email headers
* Examine attachment behavior
* Identify initial indicators of compromise
* Establish a timeline of events

Deliverables:

* Phishing Analysis Report
* IOC List
* Initial Incident Ticket

---

## Phase 2 – Host Reconnaissance

The malicious attachment performs system discovery activities.

Objectives:

* Identify executed commands
* Reconstruct process trees
* Analyze command-line activity

Deliverables:

* Process Tree Analysis
* Host Reconnaissance Findings

---

## Phase 3 – Persistence

The attacker attempts to maintain access on the system.

Objectives:

* Identify persistence mechanisms
* Investigate registry modifications
* Analyze startup behaviors

Deliverables:

* Persistence Investigation Report
* Detection Recommendations

---

## Phase 4 – Living Off The Land Activity

The attacker uses native Windows utilities to perform actions without deploying additional tools.

Objectives:

* Identify LOLBin usage
* Investigate parent-child process relationships
* Develop behavioral detections

Deliverables:

* LOLBin Analysis Report
* Detection Content

---

## Phase 5 – Internal Reconnaissance

The attacker explores the internal environment.

Objectives:

* Analyze host discovery activity
* Investigate service enumeration
* Identify reconnaissance patterns

Deliverables:

* Threat Hunting Report
* Internal Reconnaissance Findings

---

## Phase 6 – Data Collection and Staging

The attacker collects information for potential exfiltration.

Objectives:

* Identify file collection activity
* Investigate archive creation
* Determine attacker objectives

Deliverables:

* Data Staging Analysis
* IOC Updates

---

## Phase 7 – Simulated Exfiltration

The attacker attempts to move collected information outside the environment.

Objectives:

* Identify file transfer behavior
* Investigate network activity
* Develop detection logic

Deliverables:

* Exfiltration Investigation Report
* SIEM Detections

---

## Phase 8 – Defense Evasion

The attacker attempts to reduce visibility.

Objectives:

* Identify log tampering attempts
* Investigate security control interaction
* Determine attacker intent

Deliverables:

* Defense Evasion Findings
* Detection Enhancements

---

## Phase 9 – Detection Engineering

Develop SIEM detections based on observed attacker behavior.

Objectives:

* Create Splunk searches
* Build alerts
* Improve detection coverage

Deliverables:

* Detection Library
* Splunk Queries
* Alert Logic Documentation

---

## Phase 10 – Malware Analysis

Analyze the malicious attachment.

Objectives:

* Static analysis
* Dynamic analysis
* IOC extraction

Deliverables:

* Malware Analysis Report
* IOC Database
* Behavioral Summary

---

## Phase 11 – YARA Development

Develop custom signatures based on malware characteristics.

Objectives:

* Create YARA rules
* Test rule effectiveness
* Reduce false positives

Deliverables:

* YARA Rule Set
* Testing Documentation

---

## Phase 12 – SOAR Automation

Automate repetitive analyst tasks.

Objectives:

* IOC enrichment
* Automated triage
* Incident creation

Deliverables:

* Tines Workflow
* Automation Documentation

---

## Phase 13 – Incident Response

Conduct a complete incident response investigation.

Objectives:

* Scope the incident
* Identify root cause
* Recommend remediation actions

Deliverables:

* Incident Report
* Lessons Learned
* Executive Summary

---

## Expected Outcomes

By completing this project, I will demonstrate practical experience in:

* Security Monitoring
* Threat Detection
* Threat Hunting
* Incident Response
* Detection Engineering
* Malware Analysis
* SIEM Operations
* Security Automation

The final result will be a complete SOC investigation portfolio that mirrors the workflow of a modern Security Operations Center.

---

## Repository Structure

```text
SOC-Payroll-Heist/
│
├── README.md
│
├── reports/
│   ├── phishing-analysis/
│   ├── malware-analysis/
│   ├── threat-hunting/
│   ├── incident-response/
│   └── executive-summary/
│
├── detections/
│   ├── splunk/
│   ├── yara/
│   └── hunting-queries/
│
├── screenshots/
│
├── timelines/
│
├── tines/
│
├── iocs/
│
└── diagrams/
```

## Disclaimer

This project is conducted within an isolated lab environment for educational and defensive cybersecurity purposes only. All activities are performed on systems owned and controlled by the project author.
