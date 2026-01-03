# Log-004: PowerShell Execution & Script Block Logging (Event ID 4104)

## Goal
Understand PowerShell execution and how SOC analysts evaluate script activities for potential suspicious behavior.

## Environment
- Windows Virtual Machine
- Event Viewer
- PowerShell

## Investigation Steps
1. Enabled PowerShell Script Block Logging.
2. Executed several PowerShell commands (e.g., `net accounts`).
3. Opened Event Viewer and filtered for Event ID 4104.
4. Reviewed ScriptBlockText, command execution, and execution context.

## Evidence

### Screenshot 1: Executed PowerShell Commands
![PowerShell Commands](images/powershell.png)

### Screenshot 2: Event Viewer Filtered for Event ID 4104
![Filtered Event 4104](images/Event-4104.png)

### Screenshot 3: Encoded ScriptBlockText & Execution Context
![Encoded ScriptBlockText](images/encoded.png)

### Screenshot 4: Decoded ScriptBlockText
![Decoded ScriptBlockText](images/decoded.png)

## Findings
- PowerShell commands were executed by the logged-in user.
- Some commands appeared suspicious due to unusual activity (e.g.,`net accounts`).
- An **encoded command** was observed, which may indicate attempted obfuscation or malicious intent.
- No malicious payload was executed, but the behavior warrants monitoring.

## MITRE ATT&CK Mapping
- T1059.001 – PowerShell

## Lessons Learned
- PowerShell is a powerful administrative tool that can be abused by attackers.
- Script Block Logging provides critical visibility into actual commands executed.
- Always check for **encoded or obfuscated commands**, as these are often indicators of compromise.
- Context matters: not all encoded commands are malicious, but they require closer inspection.
