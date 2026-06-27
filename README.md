# Home SOC Lab: Windows Sysmon + Splunk Alert Triage

## Overview

This project is a beginner Security Operations Center lab built to practice Windows logging, endpoint telemetry, SIEM searching, authentication log review, and basic alert triage.

I used a Windows 11 Enterprise Evaluation virtual machine with Sysmon and Splunk to collect, search, and investigate Windows process creation events and failed login activity.

The goal of this project is to show hands-on cybersecurity learning, basic SOC investigation skills, and clear technical documentation.

## Project Files

The full project files are located in the `home-soc-lab` folder.

* Incident reports: `home-soc-lab/incident-reports/`
* Screenshots: `home-soc-lab/screenshots/`
* Setup notes: `home-soc-lab/setup-notes.md`
* Splunk searches: `home-soc-lab/queries/splunk-searches.md`

## Lab Goal

The goal of this lab was to:

* Build a Windows virtual machine
* Install and configure Sysmon
* Add Windows and Sysmon logs to Splunk
* Generate security-relevant Windows activity
* Search endpoint logs in Splunk
* Investigate process creation activity
* Investigate failed login activity
* Document findings in incident-style reports

## Tools Used

* Oracle VirtualBox
* Windows 11 Enterprise Evaluation
* Microsoft Sysmon
* Windows Event Viewer
* Splunk Enterprise
* PowerShell
* GitHub

## Lab Architecture

```text
Windows 11 VM
     ↓
Sysmon + Windows Security Logs
     ↓
Windows Event Logs
     ↓
Splunk
     ↓
Search + Alert Triage
     ↓
Incident Reports
```

## Skills Demonstrated

* Virtual machine setup
* Windows troubleshooting
* Sysmon installation
* Windows Event Viewer navigation
* Splunk log ingestion
* SIEM searching
* Process creation log analysis
* Parent-child process analysis
* Authentication log review
* Failed login investigation
* Command-line investigation
* Basic SOC alert triage
* Incident documentation
* Technical writing

## SIEM Validation

After configuring Splunk, I successfully ingested Windows and Sysmon logs and ran searches to confirm visibility into endpoint activity.

Example Splunk searches used:

```spl
index=* "Microsoft-Windows-Sysmon/Operational"
index=* EventCode=1
index=* net.exe
index=* powershell
index=* EventCode=4625
index=* "An account failed to log on"
```

These searches confirmed that Sysmon telemetry and Windows Security logs were searchable in Splunk and could be used for basic SOC-style investigation.

## Investigations Completed

### Suspicious PowerShell User Enumeration Investigation

I investigated a Sysmon Event ID 1 process creation event where PowerShell launched `net.exe user`.

The event showed the following parent-child process relationship:

```text
powershell.exe → net.exe user
```

The command `net user` lists local user accounts on a Windows system. In my lab, this activity was expected because I manually generated it. However, in a real SOC environment, this type of activity may be reviewed because attackers can use account discovery commands to learn more about a system after gaining access.

### Failed Login Investigation

I generated failed login attempts in the Windows VM and investigated Windows Security Event ID 4625 using Event Viewer and Splunk.

The event showed that the `socstudent` account had failed login attempts with the failure reason `Unknown user name or bad password`.

This helped me practice authentication log review, Windows Security log analysis, SIEM searching, and basic SOC triage.

## Evidence Collected

### Suspicious PowerShell User Enumeration Evidence

Key evidence from the Sysmon event:

```text
Event ID: 1
Task Category: Process Create
Image: C:\Windows\System32\net.exe
Command Line: "C:\WINDOWS\system32\net.exe" user
Parent Image: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
User: DESKTOP-N2D9N5I\socstudent
Log Source: Microsoft-Windows-Sysmon/Operational
```

### Failed Login Evidence

Key evidence from the Windows Security event:

```text
Event ID: 4625
Message: An account failed to log on
Account Name: socstudent
Failure Reason: Unknown user name or bad password
Logon Type: 2
Computer Name: DESKTOP-N2D9N5I
Log Source in Splunk: WinEventLog:Security
```

## Screenshots

Screenshots are saved in the `home-soc-lab/screenshots` folder.

Important screenshots include:

```text
windows-vm-running.png
sysmon-operational-log.png
sysmon-event-id-1-general.png
sysmon-event-id-1-net-user-parent-powershell.png
splunk-sysmon-events.png
splunk-event-id-1-search.png
splunk-net-exe-search.png
splunk-powershell-search.png
event-viewer-failed-login-4625.png
splunk-failed-login-4625.png
splunk-failed-login-details.png
splunk-failed-login-search-results.png
```

## Incident Reports

The full investigations are documented here:

```text
home-soc-lab/incident-reports/suspicious-powershell.md
home-soc-lab/incident-reports/failed-login-investigation.md
```

## Troubleshooting

During setup, I ran into repeated black-screen issues after the Windows VM installation.

I troubleshot the issue by:

* Checking VirtualBox storage settings
* Removing the Windows ISO after installation
* Confirming the virtual hard disk was attached
* Adjusting display settings
* Restarting the VM until Windows successfully reached the login screen

This helped me practice virtualization troubleshooting and careful step-by-step problem solving.

## Lessons Learned

Through this lab, I learned how Sysmon records process creation events and how those logs can be searched in Splunk.

I also learned how Windows records failed login attempts using Security Event ID 4625 and how authentication events can be reviewed in both Event Viewer and Splunk.

This project helped me practice basic SOC analyst skills, including log review, SIEM searching, event triage, command-line analysis, authentication log review, and incident documentation.

## Future Improvements

Planned improvements:

* Create a suspicious PowerShell command investigation
* Add Wazuh for endpoint monitoring
* Add MITRE ATT&CK mapping
* Add detection notes and saved Splunk searches
* Create a basic dashboard in Splunk
