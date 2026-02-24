---
Title: Exercise 07: Check Service Permissions
Objective: Use Sysinternals AccessChk to examine the permissions (ACLs) of a specific Windows service to determine which users or groups have the ability to modify, start, stop, or configure it.
Estimated Time: 20-30 minutes
Tools Needed: Windows OS, Sysinternals AccessChk.
Setup Instructions: 
1. Download the Sysinternals Suite from the Microsoft website and extract it to a convenient location (e.g., `C:\Sysinternals`).
2. Ensure you have administrative privileges to run AccessChk, as it needs to query system service permissions.
3. **Identify a Service Name:**
    *   Open the Services management console (`services.msc`).
    *   Find a service you want to inspect (e.g., "Print Spooler").
    *   Double-click the service, and in the "Service name" field (usually on the "General" tab), note its short name (e.g., `Spooler` for "Print Spooler" or `Dnscache` for "DNS Client"). We will use `Spooler` as an example.
---

## Steps

1.  **Open Command Prompt as Administrator:**
    *   Open an elevated Command Prompt (`Run as administrator`).
    *   Navigate to your Sysinternals Suite directory: `cd C:\Sysinternals`.

2.  **Execute AccessChk to Query Service Permissions:**
    *   Type the following command, replacing `"Spooler"` with the service name you identified:
        ```bash
        accesschk -c "Spooler"
        ```
    *   Press Enter.
    *   The `-c` switch tells AccessChk to check a Windows service.

3.  **Analyze the Output:**
    *   AccessChk will list the permissions for the specified service, showing which users/groups have what level of access.
    *   **Key output components:**
        *   Service name (e.g., `Spooler`)
        *   ACL entries (e.g., `RW`, `R`, `W` indicating Read/Write/Full Control).
        *   User/Group names (e.g., `NT AUTHORITY\SYSTEM`, `BUILTIN\Administrators`, `BUILTIN\Users`).
    *   **Look for Write (W) or Full Control (RW) permissions for non-administrative users/groups.** For example, if `BUILTIN\Users` or `Everyone` has `W` or `RW` permissions, it's a significant security vulnerability.

4.  **Increase Verbosity (Optional):**
    *   To see a more detailed breakdown of individual permissions, use the `-v` (verbose) switch:
        ```bash
        accesschk -vc "Spooler"
        ```
    *   This will show symbolic names for each permission (e.g., `SERVICE_START`, `SERVICE_STOP`, `SERVICE_CHANGE_CONFIG`).

## Expected Output

```
(AccessChk will output a list of users/groups and their permissions for the specified service. You should generally see System and Administrators with full control, and possibly Read access for others.)

Example Output Snippet (for "Spooler" service):
Accesschk v6.13 - Reports effective permissions for files, registry keys, services, processes, and more
Copyright (C) 2006-2023 Mark Russinovich and Aaron Margosis
Sysinternals - www.sysinternals.com

[HKLM\System\CurrentControlSet\Services\Spooler]
  RW BUILTIN\Administrators
  RW NT AUTHORITY\SYSTEM
  R  NT AUTHORITY\INTERACTIVE
  R  NT AUTHORITY\SERVICE
  R  BUILTIN\Users
```


## Reflection Questions

1.  What are some specific "Write" permissions on a service that could allow an attacker to gain control over a system?
2.  Why is it generally considered a security risk if `BUILTIN\Users` or `Everyone` has Write permissions on a critical system service?
3.  How can an attacker leverage misconfigured service permissions to establish persistence or elevate privileges on a compromised system?

## Next Steps

*   [Exercise 08: Alternate Data Streams](08-alternate-data-streams.md)
*   Further reading: [Microsoft Docs: AccessChk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk)

## Hints / Troubleshooting

*   If `accesschk` returns "No matching objects found.", ensure the service name is correct. You can verify it in `services.msc`.
*   The output can be long for some services. You can pipe it to `more` (`accesschk -c "Spooler" | more`) or redirect it to a file for easier review.
*   Always be cautious when interpreting permissions. Some services grant certain "Write" permissions to non-administrators for legitimate functions, but broader "Full Control" or "Change Configuration" for standard users is almost always a misconfiguration.
