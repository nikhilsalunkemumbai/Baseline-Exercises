---
Title: Exercise 16: Monitor Disk for Ransomware Activity with DiskMon
Objective: Learn to use Sysinternals DiskMon to monitor all disk read/write activity in real-time, helping to identify suspicious I/O patterns indicative of ransomware, data wiping, or other disk-intensive malicious activities.
Estimated Time: 30-60 minutes
Tools Needed:
*   Sysinternals DiskMon (part of Sysinternals Suite) - Download from [Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
*   A simple file editor (e.g., Notepad)
Setup Instructions:
1.  Download the Sysinternals Suite and extract it to a convenient location (e.g., `C:\Sysinternals`).
2.  Ensure you have administrative privileges. Right-click `Diskmon.exe` and select "Run as administrator".
3.  Create a test folder with a few dummy files (e.g., `C:\RansomwareTest\file1.txt`, `C:\RansomwareTest\document.docx`, `C:\RansomwareTest\image.jpg`). You can put some placeholder text/content in these files.
4.  **WARNING:** This exercise simulates ransomware-like behavior. Use a **virtual machine snapshot** if possible, or ensure the test folder is isolated, and you understand the actions taken. Never use real sensitive data.

---

## Steps

1.  **Launch DiskMon and Start Monitoring**
    *   Navigate to your Sysinternals Suite directory.
    *   Right-click `Diskmon.exe` and select "Run as administrator".
    *   DiskMon will open and immediately start displaying disk I/O activity. The window shows a continuous stream of disk reads and writes across all logical drives.
    *   The "History Depth" setting (under `Options`) can be adjusted to control how many events are displayed.
    *   *Observation:* Observe the continuous scrolling of disk activity. Notice the columns: "Time", "Process", "I/O", "Offset", "Length", "Duration", and "Path".

2.  **Simulate Ransomware-like File Activity**
    *   With DiskMon running in the background, open a Command Prompt or PowerShell window.
    *   Navigate to your test folder (e.g., `cd C:\RansomwareTest`).
    *   Perform the following actions and observe DiskMon's output after each step:
        *   **Modify a file:** Open `file1.txt` in Notepad, add some text, save, and close.
        *   **Rename a file:** `ren file1.txt encrypted_file1.txt` (or a similar renaming scheme).
        *   **Create a new file:** `echo This is a ransom note > RANSOM_NOTE.txt`
        *   **Delete files:** `del *.txt` (this will delete `RANSOM_NOTE.txt` and potentially `encrypted_file1.txt` if you didn't rename it to another extension).
    *   *Observation:* In DiskMon, pay close attention to the "I/O" column for operations like `WRITE`, `READ`, `RENAME`, `CLOSE`, and `DELETE`. Note the "Process" column to see which application initiated these operations. Look for a rapid succession of `WRITE` (encryption), `RENAME` (appending extensions), or `DELETE` operations.

3.  **Analyze DiskMon Output for Suspicious Patterns**
    *   After performing the simulated activities, you can pause DiskMon by going to `File` -> `Pause` or by clicking the pause button on the toolbar.
    *   Scroll through the recorded events.
    *   Look for a high frequency of `WRITE` operations, especially if followed by `RENAME` operations on multiple files within a short timeframe. This mimics file encryption and renaming by ransomware.
    *   Identify the processes involved in these activities. Malware would typically use its own process or inject into legitimate ones.
    *   *Question:* Can you identify the sequence of `WRITE`, `RENAME`, and `DELETE` operations corresponding to your simulated actions?

4.  **Filter Output (If applicable)**
    *   DiskMon has limited filtering capabilities compared to Procmon, but you can use external tools or analysis methods.
    *   For a very busy system, focusing on the changes in your test directory path or specific process names can help.

## Expected Output

(The actual output will be dynamic based on your system and actions. Below is a generic description of what you'd typically observe in the DiskMon GUI.)

```
DiskMon graphical user interface continuously displaying disk I/O events.
Columns will include: Time, Process, I/O, Offset, Length, Duration, Path.
When simulating ransomware-like activity, you will see bursts of:
- WRITE operations by `notepad.exe` (or `cmd.exe`/`powershell.exe`) on `C:\RansomwareTest\file1.txt`.
- RENAME operations by `cmd.exe`/`powershell.exe` changing `file1.txt` to `encrypted_file1.txt`.
- WRITE operations by `cmd.exe`/`powershell.exe` creating `C:\RansomwareTest\RANSOM_NOTE.txt`.
- DELETE operations by `cmd.exe`/`powershell.exe` on files in the test directory.
```


## Reflection Questions

1.  What specific disk I/O event types and patterns observed in DiskMon would be the strongest indicators of active ransomware encrypting files?
2.  How does the "Process" column in DiskMon help an analyst pinpoint the source (process) of suspicious disk activity?
3.  Given DiskMon's real-time nature, how might it be used as an early warning system in a security operations center (SOC), and what are its limitations for this purpose?

## Next Steps

*   [Nmap Exercises: Discover Live Hosts](../nmap/01-discover-live-hosts.md) (Transition to the next tool set)
*   Further reading: [DiskMon documentation on Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/diskmon)

## Hints / Troubleshooting

*   **High Volume of Events:** DiskMon captures *all* disk I/O. If your system is very active, the output can be overwhelming. Try to minimize background activity during this exercise.
*   **Focus on `WRITE` and `RENAME`:** These operations are key indicators of file modification and encryption.
*   **Process Name is Key:** Always correlate suspicious I/O with the process performing it. If it's an unfamiliar process performing mass modifications, it's highly suspect.
*   **Use a VM:** For any real-world simulation of ransomware, always use a sandboxed virtual machine and ensure it's disconnected from your main network.
*   **Alternative: Procmon:** While DiskMon focuses purely on disk I/O, Process Monitor (Procmon) can also capture file system events and offers more robust filtering, including the ability to filter by `Category is Write` for disk events. For a very granular analysis of file system events (including metadata changes), Procmon might be more suitable.
*   **Clean Up:** Remember to delete your `C:\RansomwareTest` folder and its contents after the exercise.
