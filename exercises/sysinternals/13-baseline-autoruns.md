---
Title: Exercise 13: Baseline System Autoruns with Autoruns
Objective: Learn to use Sysinternals Autoruns to create a comprehensive baseline of all autostarting applications, services, drivers, and scheduled tasks on a Windows system, which is crucial for detecting unauthorized persistence mechanisms installed by malware.
Estimated Time: 30-60 minutes
Tools Needed:
*   Sysinternals Autoruns (part of Sysinternals Suite) - Download from [Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
Setup Instructions:
1.  Download the Sysinternals Suite and extract it to a convenient location (e.g., `C:\Sysinternals`).
2.  Ensure you have administrative privileges for full visibility and the ability to save the complete baseline. Right-click `autoruns.exe` and select "Run as administrator".
3.  This exercise is ideally performed on a clean or known-good Windows installation to establish a reliable and trustworthy baseline for future comparisons.

---

## Steps

1.  **Launch Autoruns and Configure Display Options**
    *   Navigate to your Sysinternals Suite directory and run `autoruns.exe` as administrator.
    *   Autoruns will immediately begin scanning your system. Wait for the scan to complete (the progress bar at the bottom will disappear).
    *   Go to `Options`. For an initial baseline, consider these settings:
        *   `Hide Microsoft Entries`: Enable this to focus on third-party software, which is often more relevant for detecting suspicious items.
        *   `Verify Code Signatures`: Enable this to have Autoruns check the digital signatures of listed executables.
        *   `Hide VirusTotal Clean Entries` (optional, requires VirusTotal integration): If enabled, Autoruns will hide entries that VirusTotal reports as clean, further focusing your view on suspicious items.

2.  **Review the "Everything" Tab**
    *   The "Everything" tab consolidates all autostart entries found on your system.
    *   Scroll through the list, paying attention to entries with:
        *   Blank or "Unable to verify" in the "Publisher" or "Verified Signer" columns (especially if `Verify Code Signatures` is enabled).
        *   Unusual "Image Path" locations (e.g., temporary folders, user profile directories for system-level components).
        *   Generic or suspicious "Description" fields.
        *   Entries highlighted in pink (indicating suspicious images, often without publisher info or valid signatures).

3.  **Explore Specific Categories**
    *   Click through the various tabs (e.g., "Logon", "Explorer", "Services", "Drivers", "Scheduled Tasks") to see categorized autostart entries. This provides a more focused view of specific persistence mechanisms.
    *   *Example:* On the "Services" tab, look for services with unusual names, missing descriptions, or those not signed by a trusted publisher.

4.  **Save the Baseline Configuration**
    *   Go to `File` -> `Save...` (or press `Ctrl+S`).
    *   Choose a descriptive filename (e.g., `clean_system_baseline_YYYYMMDD.arn`) and save it in a secure location (e.g., in your `/resources` folder if you decide to keep it in the repo, or an external secure storage).
    *   Ensure the "Save As Type" is `Autoruns Data (*.arn)`. This proprietary format preserves all captured data, including signature verification and VirusTotal results (if enabled).

5.  **Simulate a Change and Compare (Optional)**
    *   Minimize Autoruns (do not close).
    *   Install a small, legitimate application (e.g., a simple utility that adds a startup entry or service).
    *   Re-open Autoruns (it will rescan automatically) or press `F5` to refresh.
    *   Go to `File` -> `Compare...` and select your previously saved `.arn` baseline file.
    *   *Observation:* Autoruns will display only the entries that have changed between the two scans. New entries will be highlighted in green, and deleted ones in red.

## Expected Output

(The actual output will be highly specific to your system. Below is a generic description of observations in Autoruns.)

```
Autoruns GUI displaying a list of autostart entries across various tabs.
Columns: Entry, Description, Publisher, Image Path, Version, Date, VirusTotal, etc.
Many entries will show "Microsoft Corporation" as Publisher and "(Verified)" in the Verified Signer column.
Suspicious entries might be highlighted in pink, have blank/unverified signers, or point to unusual paths.

When saving: A `.arn` file (e.g., "clean_system_baseline_20240224.arn") will be created, storing all scanned data.
When comparing: Autoruns will show a filtered view with new/modified entries highlighted in green.
```


## Reflection Questions

1.  Why is it critical for an incident responder to have a trusted baseline of autostart entries for critical systems?
2.  How do features like "Hide Microsoft Entries" and "Verify Code Signatures" help analysts focus on potentially malicious persistence mechanisms?
3.  If you found a suspicious entry in Autoruns, what additional information (from other Sysinternals tools or external resources) would you seek to determine if it's truly malicious?

## Next Steps

*   [Exercise 14: Capture a Registry Change with Process Monitor](../sysinternals/14-capture-a-registry-change.md)
*   Further reading: [Autoruns documentation on Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns)

## Hints / Troubleshooting

*   **False Positives:** Many legitimate applications, especially older ones, might not have proper code signatures or may install components in less-than-ideal locations. Use caution and cross-reference with online research if an entry appears suspicious.
*   **Offline Analysis:** Autoruns can analyze an offline system's autostarts by going to `File` -> `Analyze Offline System`. This is invaluable for examining compromised systems without booting them fully.
*   **Command Line (AutorunsC):** For scripting and automation, `AutorunsC.exe` provides a command-line interface to capture and export autostart data.
*   **VirusTotal Integration:** If enabled, Autoruns' VirusTotal column provides a quick reputation check. Remember to agree to the VirusTotal terms of service.
