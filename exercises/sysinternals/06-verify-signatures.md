---
Title: Exercise 06: Verify Signatures
Objective: Use Sysinternals Sigcheck to verify the digital signatures of system files, ensuring their authenticity and integrity, and identifying unsigned or potentially tampered files.
Estimated Time: 20-30 minutes
Tools Needed: Windows OS, Sysinternals Sigcheck.
Setup Instructions: 
1. Download the Sysinternals Suite from the Microsoft website and extract it to a convenient location (e.g., `C:\Sysinternals`).
2. Ensure you have administrative privileges to run Sigcheck, as it needs to access protected system directories and files.
3. **(Optional for simulation):** In a lab environment, you could potentially introduce a non-system executable and rename it to a common DLL name (e.g., `malware.dll`) in a non-standard path, and observe how Sigcheck flags it. *Never modify actual system files.*
---

## Steps

1.  **Open Command Prompt as Administrator:**
    *   Open an elevated Command Prompt (`Run as administrator`).
    *   Navigate to your Sysinternals Suite directory: `cd C:\Sysinternals` (or wherever you extracted it).

2.  **Execute Sigcheck to Scan System32 Directory:**
    *   Type the following command:
        ```bash
        sigcheck -s C:\Windows\System32
        ```
    *   Press Enter. The `-s` switch tells Sigcheck to recurse through subdirectories.

3.  **Analyze the Output:**
    *   Sigcheck will list each file found in `C:\Windows\System32` and its subdirectories, along with information about its digital signature.
    *   **Key columns to observe:**
        *   `Verified`: Should ideally be `Signed` for most Microsoft system files.
        *   `Publisher`: Should typically be `Microsoft Windows` or `Microsoft Corporation`.
        *   `Description`: Provides a description of the file.
        *   `Product`: The product the file belongs to.
    *   **Look for anomalies:**
        *   Files with `Unsigned` or `Invalid` in the `Verified` column.
        *   Files with unexpected `Publisher` names for system files.
        *   Files with `n/a` for `Verified` where you'd expect a signature (could indicate a non-executable file, but for `.exe`, `.dll` files it's suspicious).

4.  **Refine Scan for Unsigned Files (Optional):**
    *   To specifically list only unsigned or invalidly signed files, use the `-u` switch:
        ```bash
        sigcheck -u C:\Windows\System32
        ```
    *   This can help quickly pinpoint suspicious files for further investigation.

## Expected Output

```
(Sigcheck will output a detailed list of files with their signature status. Most system files should show as "Signed" by "Microsoft Windows" or "Microsoft Corporation".)

Example Output Snippet (abbreviated):
Sigcheck v2.82 - File version and signature viewer
Copyright (C) 2004-2021 Mark Russinovich
Sysinternals - www.sysinternals.com

Path:        C:\Windows\System32
toskrnl.exe
Verified:    Signed
Publisher:   Microsoft Windows
Product:     Microsoft Windows Operating System
Description: NT Kernel & System
...

Path:        C:\Windows\System32\kernel32.dll
Verified:    Signed
Publisher:   Microsoft Windows
Product:     Microsoft Windows Operating System
Description: Windows NT BASE API Client DLL
...

(If an unsigned/malicious file existed, it might look like this):
Path:        CWindows\System32\driver.sys
Verified:    Unsigned
Publisher:   n/a
Product:     n/a
Description: Some driver
...
```
![Expected output](images/exercise-06-output.png)

## Reflection Questions

1.  What is the primary purpose of a digital signature on an executable file in the context of operating system security?
2.  How could a malicious actor attempt to bypass signature verification mechanisms, and how might Sigcheck still help detect such attempts?
3.  Why is it important to run Sigcheck with administrative privileges, especially when scanning system directories?

## Next Steps

*   [Exercise 07: Check Service Permissions](07-check-service-permissions.md)
*   Further reading: [Microsoft Docs: Sigcheck](https://docs.microsoft.com/en-us/sysinternals/downloads/sigcheck)

## Hints / Troubleshooting

*   Running `sigcheck -accepteula` once will prevent the EULA dialog from appearing on subsequent runs.
*   Be patient; scanning the entire `System32` directory can take several minutes.
*   The output can be very long. You can pipe it to `more` (`sigcheck -s C:\Windows\System32 | more`) or redirect it to a file (`sigcheck -s C:\Windows\System32 > signature_report.txt`) for easier review.
*   If "Verified" shows `n/a` for `Verified Signer`, it often means the file is not an executable, DLL, or a file type that typically carries a digital signature.
