---
Title: Exercise 14: Capture a Registry Change with Process Monitor
Objective: Learn to use Sysinternals Process Monitor (Procmon) to capture and precisely identify when a specific registry key or value is modified, and by which process, which is crucial for troubleshooting configuration issues or detecting malicious activity.
Estimated Time: 30-60 minutes
Tools Needed:
*   Sysinternals Process Monitor (Procmon) - Download from [Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
Setup Instructions:
1.  Download the Sysinternals Suite and extract it to a convenient location (e.g., `C:\Sysinternals`).
2.  Ensure you have administrative privileges to capture all system events, including sensitive registry operations. Right-click `Procmon.exe` and select "Run as administrator".
3.  Identify a non-critical registry key to monitor. For this exercise, we will create a new temporary key: `HKCU\Software\BaselineTestKey`. You can use `regedit.exe` or `reg.exe` command-line tool to make changes.

---

## Steps

1.  **Launch Procmon and Prepare Filters**
    *   Run `Procmon.exe` as administrator. Procmon will immediately start capturing events, which can quickly become overwhelming.
    *   Immediately press `Ctrl+E` to stop the capture, and then `Ctrl+X` to clear the displayed events. This gives us a clean slate.
    *   Press `Ctrl+L` (or go to `Filter` -> `Filter...`) to open the Process Monitor Filter dialog.
    *   Add the following rules:
        *   `Operation` `is` `RegSetValue` `Include`
        *   `Operation` `is` `RegCreateKey` `Include`
        *   `Operation` `is` `RegDeleteKey` `Include`
        *   `Path` `contains` `BaselineTestKey` `Include`
    *   Click "Add" for each rule, then "OK". This configures Procmon to only show events related to setting or creating registry values/keys under our test key.

2.  **Start Capture and Perform Registry Modification**
    *   Press `Ctrl+E` to start capturing events again. The status bar should indicate "Capturing events..." and the display should be empty (due to our filters).
    *   Open a Command Prompt or PowerShell window as a regular user (not administrator).
    *   Execute the following command to create a new registry key and value:
        ```cmd
        reg add HKCU\Software\BaselineTestKey /v TestValue /t REG_SZ /d "Hello Baseline"
        ```
    *   *Optional:* Open `regedit.exe`, navigate to `HKCU\Software`, right-click `Software`, select `New` -> `Key`, and name it `BaselineTestKey`. Then within `BaselineTestKey`, right-click -> `New` -> `String Value`, name it `TestValue`, and set its data to `Hello Baseline`.
    *   *Observation:* Observe Procmon. You should see entries appearing in its window.

3.  **Stop Capture and Analyze Results**
    *   Once the registry modification is complete, immediately press `Ctrl+E` to stop the Procmon capture.
    *   Examine the events displayed in Procmon. You should see entries similar to `RegCreateKey` (if the key didn't exist) and `RegSetValue` related to `HKCU\Software\BaselineTestKey`.
    *   For the `RegSetValue` event, note the "Process Name" (e.g., `cmd.exe`, `powershell.exe`, or `regedit.exe`), "PID", "Path", "Operation", and "Detail" columns.
    *   Double-click the `RegSetValue` event to open "Event Properties". Review the "Event" tab for details and the "Process" tab to confirm the process information. The "Stack" tab can also show the call stack leading to the modification.

4.  **Clean Up (Optional)**
    *   Delete the created registry key:
        ```cmd
        reg delete HKCU\Software\BaselineTestKey /f
        ```
    *   Clear Procmon filters (Ctrl+L, select each filter and click "Remove", then "OK").

## Expected Output

(The actual Process Name and PID will vary. The output below describes the key information to look for in Procmon's main window and Event Properties.)

```
Process Monitor Main Window (filtered view):
- Row(s) showing:
  - Process Name: cmd.exe (or powershell.exe, regedit.exe)
  - PID: (numerical ID)
  - Operation: RegCreateKey (if key was new) or RegSetValue
  - Path: HKCU\Software\BaselineTestKey\TestValue
  - Result: SUCCESS
  - Detail: Desired Access: ..., Granted Access: ..., Length: ..., Data: Hello Baseline (for RegSetValue)

Event Properties Dialog (for RegSetValue event):
- Event Tab: Confirms Path, Operation, Result, and "Data: Hello Baseline" in Detail.
- Process Tab: Shows "Process Name", "Command Line", "User" for the modifying process.
```


## Reflection Questions

1.  How could an attacker leverage seemingly innocuous registry changes for persistence or privilege escalation, and how would Procmon help detect such activity?
2.  What is the importance of examining the "Process Name" and "Command Line" in the Event Properties when a suspicious registry modification is detected?
3.  Why is it advisable to apply specific filters in Procmon before reproducing an event you want to capture, rather than capturing everything and filtering later?

## Next Steps

*   [Exercise 15: Gather Remote System Info with PsInfo](../sysinternals/15-gather-remote-system-info.md)
*   Further reading: [Process Monitor documentation on Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)

## Hints / Troubleshooting

*   **Filter Logic:** Procmon uses an "AND" logic for filters across different attributes and "OR" logic for multiple rules within the same attribute. For example, `(Operation is RegSetValue OR Operation is RegCreateKey) AND (Path contains BaselineTestKey)`.
*   **Too Many Events:** If you still see too many events, try adding a filter for `Process Name` `is` `reg.exe`, `powershell.exe`, `cmd.exe`, or `regedit.exe` to focus on the process you used for modification.
*   **"Show Process Tree" (Ctrl+T):** This feature can help visualize the parent-child relationships of processes, which is useful if a script or another process indirectly makes the registry change.
*   **`Category is Write` filter:** For broader monitoring of changes, `Filter` -> `Category` `is` `Write` is a powerful rule to identify all write operations (file or registry). You would then combine this with other filters.
*   **`Ctrl+K` for Stack:** In "Event Properties", the "Stack" tab can show you the exact code path that led to the registry modification, which is invaluable for root cause analysis.
