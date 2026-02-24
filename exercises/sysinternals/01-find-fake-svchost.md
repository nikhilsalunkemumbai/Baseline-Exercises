---
Title: Exercise 01: Find Fake svchost
Objective: Use Process Explorer to identify potentially malicious `svchost.exe` processes by verifying their legitimate path (`C:\Windows\System32`).
Estimated Time: 20-30 minutes
Tools Needed: Windows OS, Sysinternals Process Explorer.
Setup Instructions: 
1.  Download the Sysinternals Suite from the Microsoft website and extract it to a convenient location (e.g., `C:\Sysinternals`).
2.  Ensure you have administrative privileges to run Process Explorer effectively.
3.  (Optional for simulation): You can place a dummy executable named `svchost.exe` in a non-standard directory (e.g., `C:\Temp\svchost.exe`) for a more realistic scenario. *Do not run this fake executable.*
---

## Steps

1.  **Launch Process Explorer:**
    *   Navigate to the directory where you extracted the Sysinternals Suite.
    *   Right-click `procexp.exe` and select "Run as administrator" to ensure full functionality.

2.  **Locate `svchost.exe` Processes:**
    *   In the main Process Explorer window, scroll through the process list and locate all instances of `svchost.exe`. There will typically be multiple legitimate instances running.

3.  **Verify Image Path for Each `svchost.exe`:**
    *   For each `svchost.exe` process:
        *   Hover your mouse cursor over the `svchost.exe` process name in the list. A tooltip will appear showing the full path to the executable.
        *   Alternatively, right-click the `svchost.exe` process and select "Properties". Go to the "Image" tab. The "Path" field will show the full path.
    *   **Legitimate `svchost.exe` instances should always have the path `C:\Windows\System32\svchost.exe`**.

4.  **Identify Anomalies:**
    *   Look for any `svchost.exe` processes whose path is *not* `C:\Windows\System32\svchost.exe`. These are highly suspicious and could indicate malware masquerading as a legitimate system process.

5.  **Examine Suspicious Processes (Optional):**
    *   If you find a suspicious `svchost.exe`, you can further investigate by checking its "Company Name", "Verified Signer" (in properties), and loaded DLLs (lower pane) for non-Microsoft entries or unsigned modules.

## Expected Output

```
(You will see a list of svchost.exe processes. Legitimate ones will show their path as C:\Windows\System32\svchost.exe. If a fake one exists, its path will be different.)

Example Process Explorer Snippet (hovering over a legitimate svchost.exe):
--------------------------------------------------
svchost.exe (PID: 1234)
Path: C:\Windows\System32\svchost.exe
Command line: C:\Windows\System32\svchost.exe -k LocalService -p
User: NT AUTHORITY\LOCAL SERVICE
...
--------------------------------------------------

(If a fake svchost.exe exists, its path would appear differently, for example):
--------------------------------------------------
svchost.exe (PID: 5678)
Path: C:\Users\User\Downloads\svchost.exe  <-- Suspicious path!
Command line: C:\Users\User\Downloads\svchost.exe
User: DESKTOP-ABC\User
...
--------------------------------------------------
```


## Reflection Questions

1.  Why do malware authors often name their processes `svchost.exe` or similar system process names?
2.  Besides the file path, what other indicators in Process Explorer might suggest a `svchost.exe` process is malicious?
3.  What are the potential dangers of blindly terminating a `svchost.exe` process without proper investigation?

## Next Steps

*   [Exercise 02: Disable Malware Auto-start](02-disable-malware-auto-start.md)
*   Further reading: [Microsoft Docs: Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer)

## Hints / Troubleshooting

*   Ensure Process Explorer is run as administrator to view details for all processes.
*   The "Path" column can be added to the main display via `View > Select Columns... > Process Image Tab`.
*   A common legitimate location for `svchost.exe` is `C:\Windows\System32`. Any other location is highly suspicious.
