# Mini SOC Project: Host-Based Persistence Investigation

## Incident Summary
Multiple persistence mechanisms were identified on a Windows host following suspicious outbound network activity. The activity was analyzed to determine whether it represented malicious behavior.

## Environment
- Windows VM
- Event Viewer
- Windows Defender Firewall
- Task Scheduler
- Windows Registry Editor

## Network Activity Analysis
- Outbound connections were identified via Event ID 5156
- Process initiating the connection was reviewed
- Destination IP, port, and protocol were analyzed

## Scheduled Task Analysis
- A scheduled task was created (Event ID 4698)
- Task executed PowerShell using stealth flags
- Task behavior resembled persistence mechanisms
- Task deletion activity was observed (Event ID 4699)

## Registry Persistence Analysis
- A registry Run key was modified (Event ID 4657)
- PowerShell execution was configured at user logon
- Registry modification correlated with prior persistence activity

## Correlation and Timeline
The sequence of network activity followed by multiple persistence mechanisms indicated deliberate and coordinated behavior rather than isolated events.

## Analyst Assessment
The presence of multiple persistence mechanisms combined with stealthy PowerShell execution suggests malicious intent. Further investigation and containment would be recommended.

## MITRE ATT&CK Mapping
- T1071 – Application Layer Protocol
- T1053.005 – Scheduled Task
- T1547.001 – Registry Run Keys

## Lessons Learned
- Persistence mechanisms often occur in clusters
- PowerShell stealth flags are strong indicators of malicious activity
- Correlating logs across domains increases detection confidence

