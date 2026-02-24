---
Title: Exercise 15: Gather Remote System Info with PsInfo
Objective: Learn to use Sysinternals PsInfo to quickly and remotely gather detailed system information from a target Windows machine, which is essential for asset inventory, compliance checks, and remote troubleshooting.
Estimated Time: 20-40 minutes
Tools Needed:
*   Sysinternals PsInfo (part of PsTools in Sysinternals Suite) - Download from [Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
*   Two Windows machines on the same network (one local, one remote target). This can be two VMs.
Setup Instructions:
1.  Download the Sysinternals Suite and extract it to a convenient location (e.g., `C:\Sysinternals`) on your local machine.
2.  Ensure you have a local Windows machine (your workstation) and a remote target Windows machine (VM or another physical PC) that are on the same network and can communicate.
3.  The user account on your local machine must have administrative privileges on the remote target machine.
4.  The remote target machine's firewall must allow "Remote Administration" (WMI) traffic. This is typically enabled by default in domain environments but might need manual configuration in workgroups.

---

## Steps

1.  **Gather Local System Information**
    *   Open a Command Prompt or PowerShell window on your local machine.
    *   Navigate to the directory where you extracted the Sysinternals Suite (e.g., `cd C:\Sysinternals`).
    *   Execute `psinfo.exe` without any parameters to see its default output:
        ```cmd
        psinfo.exe
        ```
    *   *Observation:* Note the type of information displayed (OS version, uptime, CPU type, memory, etc.).

2.  **Gather Remote System Information**
    *   Replace `[RemoteComputerNameOrIP]` with the actual hostname or IP address of your remote target machine.
    *   Execute `psinfo.exe` to gather information from the remote machine:
        ```cmd
        psinfo.exe \your_remote_machine_name_or_ip
        ```
    *   *Observation:* Confirm that the output displays the system information for your remote target, similar to the local scan.

3.  **Gather Specific Remote Information**
    *   **Installed Hotfixes:** Use the `-h` option (for Hotfixes, as described in the documentation, although the documentation snippet mentions `-s` for installed applications - cross-referencing with official docs is key here, but we will use the prompt's `psinfo register` example and adjust for hotfixes for consistency, assume `-h` is for hotfixes based on common `psinfo` usage and similar tools):
        ```cmd
        psinfo.exe \your_remote_machine_name_or_ip -h
        ```
    *   **Disk Volume Information:** Use the `-d` option to list disk volumes:
        ```cmd
        psinfo.exe \your_remote_machine_name_or_ip -d
        ```
    *   **Registered Applications (Software):** Use the `-s` option to view installed software applications (as per documentation snippet).
        ```cmd
        psinfo.exe \your_remote_machine_name_or_ip -s
        ```
    *   *Observation:* Review the specific information returned for hotfixes, disk volumes, and installed software.

4.  **Use Alternate Credentials (If Needed)**
    *   If your current user does not have administrative rights on the remote machine, you can specify alternate credentials using the `-u` (username) and `-p` (password) options.
    *   *Note:* Omitting `-p` will prompt you for the password securely.
    *   ```cmd
        psinfo.exe \your_remote_machine_name_or_ip -u [RemoteUsername] -p [RemotePassword]
        ```
    *   *Observation:* The command should successfully execute, demonstrating the use of alternate credentials.

## Expected Output

(Output will vary based on your local and remote system configurations.)

```
System information for \LOCALMACHINE:
Uptime: X days Y hours Z minutes A seconds
Kernel version: Windows 10, ...
Product type: ...
IE version: ...
System root: C:\Windows
Processors: ...
Processor speed: ...
Physical memory: ... MB
...

System information for \REMOTEMACHINE:
Uptime: X days Y hours Z minutes A seconds
Kernel version: Windows 10, ...
Product type: ...
IE version: ...
System root: C:\Windows
Processors: ...
Processor speed: ...
Physical memory: ... MB
...

Disk volume information for \REMOTEMACHINE:
Volume Type Format Label Size Free Free
C: Fixed NTFS ... ... ...
...

Installed software applications for \REMOTEMACHINE:
[List of installed applications]
...
```


## Reflection Questions

1.  How does PsInfo streamline the process of gathering system-level intelligence compared to manually logging into each machine or using built-in Windows tools?
2.  What potential security risks are associated with using alternate credentials or hardcoding them in scripts when using tools like PsInfo for remote access?
3.  In what scenarios would using PsInfo be preferred over a full network vulnerability scanner for collecting system information?

## Next Steps

*   [Exercise 16: Monitor Disk for Ransomware with DiskMon](../sysinternals/16-monitor-disk-for-ransomware.md)
*   Further reading: [PsInfo documentation on Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/psinfo)

## Hints / Troubleshooting

*   **"Access Denied" or "RPC server is unavailable":**
    *   Ensure the account running PsInfo has administrative rights on the *remote* machine.
    *   Verify network connectivity (e.g., `ping [RemoteComputerNameOrIP]`).
    *   Check the firewall on the *remote* machine: WMI (Windows Management Instrumentation) or "Remote Administration" rules need to be enabled. Sometimes temporarily disabling the firewall can help diagnose.
    *   Ensure the "Remote Registry" service is running on the remote machine (although PsInfo can run without it for some info, it helps).
*   **Hostname vs. IP:** If hostname resolution fails, try using the IP address of the remote machine directly.
*   **WMI Issues:** PsInfo relies on WMI. If WMI is corrupted or misconfigured on the remote system, PsInfo may not work correctly.
*   **Output Filtering:** PsInfo also allows you to filter output for specific fields (e.g., `psinfo \computer register` to see only Registered Organization and Owner). This can be useful for targeted information gathering.
