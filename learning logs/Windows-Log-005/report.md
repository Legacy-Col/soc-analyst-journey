Goal
Understand Host based connections and how SOC analysis outbound trafic

## Environment
- Windows Vm
- Event Viewer
- Windows Defender Firewall

## Investigation Steps
- Enabled Firewall Logging
- Enabled Audit policy for Firewall
- Generated Network activities using powershell with commands such as (`ping and nslookup`).
- Filtered for Event ID 5156

## Evidence 
### Screenshot 1: Powershell Command and Executions carried out
- ![]()

### Screenshot 2: Filtered Events ID 5156
- ![]()

### Screenshot 3: Details for the Event Log 5156
- ![]()

## Findings
- Process Initiated Outbound connection
- Destination IP and Port were discovered
- Discovered which protocol was used

## MITRE ATT&CK Mapping
- T1043 - Commonly used port
- T1071 - Application layer protocol

## Lessons Learned
- Network Logs provide visibility on the type of connections that where made
- Process context are critical when evaluating connections
