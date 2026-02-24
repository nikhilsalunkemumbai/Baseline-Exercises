---
Title: Exercise 05: Remote Command
Objective: Execute a command (`ipconfig`) on a remote Windows machine using Sysinternals PsExec, demonstrating remote administration capabilities.
Estimated Time: 20-30 minutes
Tools Needed: Windows OS (local and remote machines), Sysinternals PsExec.
Setup Instructions: 
1. Download the Sysinternals Suite from the Microsoft website and extract it to a convenient location (e.g., `C:\Sysinternals`).
2. **Local Machine:** This is the machine from which you will execute PsExec.
    *   Copy `PsExec.exe` from the extracted Sysinternals folder to a directory included in your system's PATH environment variable (e.g., `C:\Windows\System32`) or ensure you navigate to the Sysinternals folder in your command prompt.
3. **Remote Machine:** This is the target machine where the command will be executed.
    *   Ensure both the local and remote machines are on the same network and can communicate with each other (e.g., you can ping the remote machine's IP address from the local machine).
    *   **Firewall Configuration:** The remote machine's firewall must allow incoming connections for File and Printer Sharing (SMB, TCP ports 139, 445), and potentially remote service management. For a lab environment, temporarily disabling the firewall on the remote machine might simplify the setup, but *do not do this in a production environment*.
    *   You need administrative credentials (username and password) for the remote machine.
---

## Steps

1.  **Open Command Prompt on Local Machine:**
    *   Open an elevated Command Prompt (`Run as administrator`) on your local machine.

2.  **Execute `ipconfig` Remotely using PsExec:**
    *   Type the following command, replacing `<RemoteMachineIP>` with the actual IP address or hostname of your remote machine, and `<AdminUsername>`/`<AdminPassword>` with the administrative credentials for the remote machine:
        ```bash
        psexec \<RemoteMachineIP> -u <AdminUsername> -p <AdminPassword> ipconfig /all
        ```
    *   **Example:** If your remote machine's IP is `192.168.1.10` and admin username is `Administrator` with password `Password123!`:
        ```bash
        psexec \192.168.1.10 -u Administrator -p Password123! ipconfig /all
        ```
    *   If you omit `-p <AdminPassword>`, PsExec will prompt you for the password securely.

3.  **Analyze the Output:**
    *   The `ipconfig /all` output from the remote machine will be displayed directly in your local Command Prompt window.
    *   Verify that the IP addresses, hostname, and other network configuration details displayed belong to the remote machine, not your local one.

4.  **Confirm Remote Execution (Optional):**
    *   On the remote machine, quickly open Task Manager and look for a `conhost.exe` process (or `cmd.exe` if no console window is configured) that might briefly appear when `ipconfig` is executed. PsExec starts a temporary service and process to run the command.

## Expected Output

```
(The output displayed in your local command prompt will be the ipconfig /all results from the remote machine.)

PsExec v2.34 - Execute processes remotely
Copyright (C) 2001-2021 Mark Russinovich and Bryce Cogswell
Sysinternals - www.sysinternals.com

Microsoft Windows IP Configuration

   Host Name . . . . . . . . . . . . : REMOTE-PC
   Primary Dns Suffix  . . . . . . . : 
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . : 
   Description . . . . . . . . . . . : Realtek PCIe GbE Family Controller
   Physical Address. . . . . . . . . : 00-11-22-33-44-55
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv4 Address. . . . . . . . . . . . : 192.168.1.10(Preferred) 
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.1.1
   DNS Servers . . . . . . . . . . . : 192.168.1.1
   NetBIOS over Tcpip. . . . . . . . : Enabled
... (rest of ipconfig /all output from remote machine)
ipconfig exited on <RemoteMachineIP> with error code 0.
```
![Expected output](images/exercise-05-output.png)

## Reflection Questions

1.  How does PsExec achieve remote execution without requiring a pre-installed agent on the remote machine?
2.  What are the primary security implications of a compromised PsExec utility or exposed administrative credentials for a remote machine?
3.  In a security incident, how might PsExec be used by an attacker for lateral movement within a network?

## Next Steps

*   [Exercise 06: Verify Signatures](06-verify-signatures.md)
*   Further reading: [Microsoft Docs: PsExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)

## Hints / Troubleshooting

*   If PsExec fails with "Access Denied", ensure the username and password are correct and that the user has administrative privileges on the remote machine. Also, check the remote machine's firewall.
*   "The network path was not found" usually indicates a network connectivity issue or a firewall blocking SMB traffic.
*   Make sure you are using the correct backslashes (``) for the remote machine path.
*   In a production environment, always use PsExec with caution and only on authorized systems.
