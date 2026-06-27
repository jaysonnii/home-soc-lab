# Splunk Searches

## Sysmon Searches

```spl
index=* "Microsoft-Windows-Sysmon/Operational"
```

Purpose: Confirm Sysmon logs are being ingested into Splunk.

```spl
index=* EventCode=1
```

Purpose: Find Sysmon process creation events.

```spl
index=* net.exe
```

Purpose: Search for `net.exe` activity.

```spl
index=* powershell
```

Purpose: Search for PowerShell-related activity.

---

## Failed Login Searches

```spl
index=* EventCode=4625
```

Purpose: Search for Windows failed login events.

```spl
index=* "An account failed to log on"
```

Purpose: Search for failed login events using the event message.

```spl
index=* 4625
```

Purpose: Broad search for events containing Event ID 4625.
