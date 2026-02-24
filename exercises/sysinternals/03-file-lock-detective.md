---
Title: Exercise 03: File Lock Detective
Objective: Identify which process (or processes) has an open handle to a specific file or directory using Sysinternals Handle, a valuable skill for troubleshooting "file in use" errors.
Estimated Time: 15-25 minutes
Tools Needed: Windows OS, Sysinternals Handle, a file currently locked by a process.
Setup Instructions: 
1. Download the Sysinternals Suite from the Microsoft website and extract it to a convenient location (e.g., `C:\Sysinternals`).
2. Ensure you have administrative privileges to run Handle effectively.
3. **Create and Lock a File:**
    *   Create a new text file on your desktop named `locked_document.txt`.
    *   Open `locked_document.txt` in a text editor (e.g., Notepad, VS Code, Word). *Do not save and close the file; keep it open and unsaved (or just open) to simulate a lock.*
---

## Steps

1.  **Open Command Prompt as Administrator:**
    *   Press `Win + R`, type `cmd`, and press `Ctrl + Shift + Enter` to open an elevated Command Prompt.
    *   Navigate to your Sysinternals Suite directory: `cd C:\Sysinternals` (or wherever you extracted it).

2.  **Use Handle to Search for the Locked File:**
    *   In the Command Prompt, type the following command, replacing `C:\Users\<username>\Desktop\locked_document.txt` with the actual path to your locked file:
        ```bash
        handle -a "C:\Users\<username>\Desktop\locked_document.txt"
        ```
    *   Press Enter.

3.  **Analyze the Output:**
    *   The `handle` utility will list any processes that have an open handle to the specified file.
    *   The output will typically show:
        *   The process name (e.g., `notepad.exe`, `Code.exe`).
        *   Its Process ID (PID).
        *   The type of handle (e.g., `File`).
        *   The path to the locked file.

4.  **Verify the Locking Process:**
    *   Confirm that the process identified by Handle is indeed the application you used to open and "lock" the file (e.g., Notepad).

## Expected Output

```
(The output from the `handle` command will list the process(es) holding a lock on your specified file.)

Example Output:
-----------------------------------------------------------------------------
NirSoft Handle.exe v3.0 -- Handle analysis utility
Copyright (c) 1996-2023 Mark Russinovich
Sysinternals - www.sysinternals.com
-----------------------------------------------------------------------------

notepad.exe        pid: 7890   type: File   2F8: C:\Users\YourUser\Desktop\locked_document.txt
```


## Reflection Questions

1.  How does an operating system typically manage file locks, and why are they necessary?
2.  What is the significance of the Process ID (PID) in the output of the `handle` command when troubleshooting?
3.  In a security incident, how might `handle` be used to investigate a malicious process that is preventing access to critical files?

## Next Steps

*   [Exercise 04: Monitor Process Activity](04-monitor-process-activity.md)
*   Further reading: [Microsoft Docs: Handle](https://docs.microsoft.com/en-us/sysinternals/downloads/handle)

## Hints / Troubleshooting

*   Ensure you run the Command Prompt as administrator, as `handle` might require elevated privileges to inspect handles held by other user processes.
*   Make sure the file path in your `handle` command is enclosed in double quotes if it contains spaces.
*   If no process is listed, ensure the file is genuinely locked. Sometimes applications release file handles even if the file is still open in their UI.
*   Closing the application that locked the file should immediately make `handle` report no open handles for that file.
