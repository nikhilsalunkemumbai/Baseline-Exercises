---
Title: Exercise 09: Run as Another User
Objective: Launch an application with different user credentials using Sysinternals ShellRunas, simulating scenarios where specific tasks require elevated or alternate user contexts without logging off.
Estimated Time: 15-25 minutes
Tools Needed: Windows OS, Sysinternals ShellRunas, a secondary user account.
Setup Instructions: 
1. Download the Sysinternals Suite from the Microsoft website and extract it to a convenient location (e.g., `C:\Sysinternals`).
2. Ensure you have administrative privileges for some steps.
3. **Create a Secondary User Account:**
    *   On your Windows system, create a new **Standard User** account (e.g., "TestUser") if you don't have one. Set a password for this account.
    *   You will use this account's credentials to launch a program.
4. **Register ShellRunas:**
    *   Open an elevated Command Prompt (`Run as administrator`).
    *   Navigate to your Sysinternals Suite directory: `cd C:\Sysinternals`.
    *   Execute the command to register ShellRunas: `shellrunas.exe /reg`
    *   You should see a message confirming that "Run as another user..." has been added to the context menu.
---

## Steps

1.  **Locate an Application to Launch:**
    *   Open File Explorer and navigate to an executable file (e.g., `notepad.exe` in `C:\Windows\System32`, or `calc.exe`).

2.  **Right-Click and Select "Run as another user...":**
    *   Right-click on the executable file (e.g., `notepad.exe`).
    *   From the context menu, select "Run as another user...".

3.  **Enter Alternate Credentials:**
    *   A dialog box titled "Run as another user" will appear.
    *   Enter the username and password for the secondary user account you created in the Setup (e.g., "TestUser" and its password).
    *   Click "OK".

4.  **Verify the Application's User Context:**
    *   The application (e.g., Notepad) should launch.
    *   Open Sysinternals Process Explorer (run as administrator).
    *   Locate the newly launched application's process (e.g., `notepad.exe`).
    *   In the "User Name" column, you should see the secondary user account (e.g., `YourComputerName\TestUser`) instead of your currently logged-in user.
    *   Alternatively, right-click the process in Process Explorer, select "Properties", and go to the "Image" tab. The "User" field will confirm the user account.

5.  **Unregister ShellRunas (Cleanup):**
    *   Open an elevated Command Prompt (`Run as administrator`).
    *   Navigate to your Sysinternals Suite directory: `cd C:\Sysinternals`.
    *   Execute: `shellrunas.exe /unreg`
    *   This removes the "Run as another user..." option from the context menu.

## Expected Output

```
(After right-clicking an executable and selecting "Run as another user...", a credentials prompt will appear. After entering valid credentials, the application will launch. Process Explorer will show the application running under the specified alternate user.)

Example of ShellRunas Credentials Prompt:
------------------------------------------
Run as another user
User name: TestUser
Password:  ********
Domain:    (YourComputerName)
------------------------------------------

Example in Process Explorer (for notepad.exe launched as TestUser):
Process Name   PID   User Name
-------------- ----- ----------------------
notepad.exe    8765  YOURCOMPUTERNAME\TestUser
...
```


## Reflection Questions

1.  In what scenarios (e.g., security, administration) would launching an application as another user be a necessary or beneficial task?
2.  How does ShellRunas differ from the standard `runas` command-line utility in terms of user experience?
3.  What potential security risks are associated with using `Run as another user` if the secondary account has elevated privileges?

## Next Steps

*   [Exercise 10: Analyze Memory Usage](10-analyse-memory-usage.md)
*   Further reading: [Microsoft Docs: ShellRunas](https://docs.microsoft.com/en-us/sysinternals/downloads/shellrunas)

## Hints / Troubleshooting

*   If "Run as another user..." does not appear in the context menu, ensure you ran `shellrunas.exe /reg` from an elevated Command Prompt.
*   If the application fails to launch or immediately closes, double-check the username and password, and verify the secondary account has permissions to execute the application.
*   Some applications might behave differently or fail to launch when run under a different user context, especially if they expect specific user profile settings or administrative privileges.
