## Troubleshooting

During setup, I resolved a VirtualBox black-screen issue by checking storage, removing the installation ISO after Windows installed, and adjusting display setting0s.



# Home SOC Lab: Windows Sysmon + Alert Triage

## Overview

This project is a beginner Security Operations Center lab built to practice Windows logging, endpoint telemetry, and basic alert triage.

I used a Windows 11 Enterprise Evaluation virtual machine with Sysmon to collect process creation events and investigate command-line activity through Windows Event Viewer.

The goal of this project is to show hands-on cybersecurity learning, basic SOC investigation skills, and clear technical documentation.

## Lab Goal

The goal of this lab was to:

* Build a Windows virtual machine
* Install and configure Sysmon
* Generate security-relevant Windows activity
* Review Sysmon logs in Event Viewer
* Investigate a process creation event
* Document findings in an incident-style report

## Tools Used

* Oracle VirtualBox
* Windows 11 Enterprise Evaluation
* Microsoft Sysmon
* Windows Event Viewer
* PowerShell
* GitHub

## Lab Architecture

```text
Windows 11 VM
     ↓
Sysmon
     ↓
Windows Event Logs
     ↓
Event Viewer
     ↓
Incident Report
```

## Skills Demonstrated

* Virtual machine setup
* Windows troubleshooting
* Sysmon installation
* Windows Event Viewer navigation
* Process creation log analysis
* Parent-child process analysis
* Command-line investigation
* Basic SOC alert triage
* Incident documentation
* Technical writing

## Investigation Completed

### Suspicious PowerShell User Enumeration Investigation

I investigated a Sysmon Event ID 1 process creation event where PowerShell launched `net.exe user`.

The event showed the following parent-child relationship:

```text
powershell.exe → net.exe user
```

The command `net user` lists local user accounts on a Windows system. In my lab, this activity was expected because I manually generated it. However, in a real SOC environment, this type of activity may be reviewed because attackers can use account discovery commands to learn more about a system after gaining access.

## Evidence Collected

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

## Screenshots

Screenshots are saved in the `screenshots` folder.

Important screenshots include:

```text
windows-vm-running.png
sysmon-operational-log.png
sysmon-event-id-1-general.png
sysmon-event-id-1-net-user-parent-powershell.png
```

## Incident Report

The full investigation is documented here:

```text
incident-reports/suspicious-powershell.md
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

Through this lab, I learned how Sysmon records process creation events and how those logs can be used to investigate activity on a Windows endpoint.

I also learned how important parent-child process relationships are in security investigations. In this case, seeing PowerShell launch `net.exe user` helped me understand how analysts can connect user activity, command-line behavior, and possible discovery techniques.

This project helped me practice the type of basic log analysis and documentation expected in entry-level cybersecurity and SOC analyst work.

## Future Improvements

Planned improvements:

* Add Splunk for SIEM searching
* Add Wazuh for endpoint monitoring
* Create a failed login investigation
* Create a suspicious PowerShell command investigation
* Add MITRE ATT&CK mapping
* Add more detection notes and search queries
