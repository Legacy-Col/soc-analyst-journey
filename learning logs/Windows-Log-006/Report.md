## Goal
Understand how SOC analysts detect and analyze persistence mechanisms using Windows Scheduled Task logs.

## Environment
- Windows VM
- Event Viewer
- Task Scheduler

## Investigation Steps
- Enabled audit policy for scheduled task activity
- Created a scheduled task using Command Prompt with administrative privileges
- Filtered Security logs for Event ID 4698 (Scheduled task created)
- Filtered Security logs for Event ID 4699 (Scheduled task deleted)
- Investigated task content, trigger type, run-as context, and execution properties

## Evidence

### Screenshot 1: Scheduled Task Creation via Command Prompt
![Task Created](images/cmd-prompt.png)

### Screenshot 2: Filtered Event ID 4698
![Event 4698](images/event-4698.png)

### Screenshot 3: Event ID 4698 Task Details
![Task Details](images/detail-4698.png)

### Screenshot 4: Filtered Event ID 4699
![Event 4699](images/event-4699.png)

### Screenshot 5: Event ID 4699 Deletion Details
![Deletion Details](images/detail-4699.png)

## Findings
- A scheduled task was created on the system using administrative privileges
- The task was configured to execute automatically upon user logon
- The task executed PowerShell using the flags `-nop`, `-w hidden`, and `-c`, indicating stealthy and non-interactive execution
- Task configuration and execution behavior were consistent with persistence techniques

## MITRE ATT&CK Mapping
- **T1053.005** – Scheduled Task / Job: Scheduled Task

## Lessons Learned
- Scheduled tasks are a commonly abused persistence mechanism
- Attackers leverage PowerShell flags such as `-nop`, `-w hidden`, and `-c` to reduce visibility
- Task triggers and execution context are critical when identifying persistence
- Persistence activity must be correlated with prior authentication and execution events
