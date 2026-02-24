---
Title: Exercise 12: Check Loaded DLLs in Process Explorer
Objective: Learn to use Sysinternals Process Explorer to analyze dynamically linked libraries (DLLs) loaded by a process and identify unsigned, unverified, or suspicious DLLs that might indicate malware activity or system compromise.
Estimated Time: 30-60 minutes
Tools Needed:
*   Sysinternals Process Explorer (part of Sysinternals Suite) - Download from [Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
*   A running Windows process (e.g., a web browser, a simple game, or an application you suspect might be compromised)
Setup Instructions:
1.  Download the Sysinternals Suite and extract it to a convenient location (e.g., `C:\Sysinternals`).
2.  Ensure you have administrative privileges to inspect all processes and their loaded modules. Right-click `procexp.exe` and select "Run as administrator".
3.  Open a few applications to simulate a typical user environment and provide target processes for inspection.
---

## Steps

1.  **Launch Process Explorer and Configure Display**
    *   Navigate to your Sysinternals Suite directory and run `procexp.exe` as administrator.
    *   Go to `Options` -> `Verify Image Signatures` and ensure it is checked. This enables signature verification for loaded images.
    *   Go to `View` -> `Select Columns...`. In the "DLL tab", check "Verified Signer" and "Company Name" if they are not already selected, then click "OK". These columns are crucial for identifying legitimate versus suspicious DLLs.

2.  **Activate DLL View for a Target Process**
    *   In the upper pane (process list), select a running process (e.g., `chrome.exe`, `firefox.exe`, or `notepad.exe`).
    *   Press `Ctrl+D` or click `View` -> `Show Lower Pane` (if not already visible) and then `View` -> `DLLs` to switch the lower pane to DLL View.
    *   *Observation:* The lower pane should now display a list of all DLLs and other memory-mapped files loaded by the selected process, along with their path, company name, and verified signer status.

3.  **Inspect Loaded DLLs for Anomalies**
    *   In the lower pane (DLL View), observe the "Verified Signer" and "Company Name" columns.
    *   Scroll through the list and look for:
        *   **Blank "Verified Signer" entries:** This means the DLL is not digitally signed or its signature could not be verified. While many legitimate applications use unsigned DLLs, this is a flag for deeper investigation, especially for critical system processes.
        *   **"Unable to verify" in "Verified Signer":** Indicates a problem with the signature or an unsigned file.
        *   **Unusual "Company Name" for its path:** For example, a DLL in `C:\Windows\System32` that is not signed by "Microsoft Corporation".
        *   **Unusual file paths:** A DLL loaded from a temporary directory, user profile, or an unexpected location.
        *   **Generic/Nondescript "Description" or "Company Name":** Malware often uses vague descriptions.
    *   *Action:* For any suspicious DLL, right-click it and select "Search Online..." to perform a web search for the DLL name and path. This can help determine its legitimacy.

4.  **Sort and Filter for Efficiency**
    *   Click on the "Verified Signer" column header in the lower pane to sort the DLLs. This groups unsigned or unverified DLLs together, making them easier to spot.
    *   Alternatively, sort by "Company Name" to quickly identify non-Microsoft DLLs.
    *   *Tip:* In a real investigation, you might look for common malware locations (e.g., `C:\Users\<username>\AppData\Local\Temp`) in the "Path" column.

## Expected Output

(The actual output will vary depending on your running processes and loaded DLLs. Below is a generic description of observations in Process Explorer.)

```
Process Explorer GUI with upper pane showing processes and lower pane showing DLLs for the selected process.
DLL View columns visible: Path, Company Name, Verified Signer.
Many DLLs will show "Microsoft Corporation" as the Company Name and "(Verified)" or "Microsoft Windows" as the Verified Signer.
Potentially suspicious entries might have blank "Verified Signer", "(Unable to verify)", or non-standard Company Names/Paths.
```
![Expected output](images/exercise-12-output.png)

## Reflection Questions

1.  Why is analyzing DLLs a crucial step in identifying malware or system compromise?
2.  What is the significance of a DLL being loaded from an unusual path (e.g., a user's `AppData` folder) compared to `C:\Windows\System32`?
3.  When investigating a suspicious DLL, beyond "Verified Signer" and "Company Name," what other columns or features in Process Explorer would you examine for more clues?

## Next Steps

*   [Exercise 13: Baseline System Autoruns with Autoruns](../sysinternals/13-baseline-autoruns.md)
*   Further reading: [Process Explorer documentation on Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/process-explorer)

## Hints / Troubleshooting

*   **Many DLLs:** Don't be overwhelmed by the large number of DLLs. Most are legitimate system components. Focus on outliers based on signer, company, or path.
*   **Performance:** Enabling "Verify Image Signatures" can slow down Process Explorer slightly due to the checks.
*   **VirusTotal Integration:** For a more advanced check, Process Explorer also has a VirusTotal integration (via `Options` -> `VirusTotal.com` -> `Check VirusTotal.com`). This can provide an immediate reputation check for loaded modules.
*   **32-bit vs. 64-bit:** On 64-bit Windows, Process Explorer will show both 32-bit and 64-bit processes and their respective DLLs. It often launches a 32-bit helper for inspecting 32-bit processes.
