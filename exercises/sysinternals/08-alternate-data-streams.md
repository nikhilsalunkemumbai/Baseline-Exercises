---
Title: Exercise 08: Alternate Data Streams
Objective: Use Sysinternals Streams to detect and list Alternate Data Streams (ADS) associated with a file, which can sometimes be used by malware to hide data or execute malicious code.
Estimated Time: 15-25 minutes
Tools Needed: Windows OS, Sysinternals Streams.
Setup Instructions: 
1. Download the Sysinternals Suite from the Microsoft website and extract it to a convenient location (e.g., `C:\Sysinternals`).
2. Ensure you have administrative privileges (recommended for full access to all files).
3. **Create a file with an Alternate Data Stream (ADS):**
    *   Open an elevated Command Prompt (`Run as administrator`).
    *   Create a temporary directory if you don't have one: `mkdir C:\Temp`
    *   Execute the following command to create a file named `ads_test.txt` and attach a hidden stream named `hidden_stream` to it:
        ```bash
        echo "This is some hidden data in an ADS." > C:\Temp\ads_test.txt:hidden_stream
        ```
    *   Verify that `C:\Temp\ads_test.txt` appears to be a normal text file (e.g., its size might be 0 bytes or the size of its main content).
---

## Steps

1.  **Open Command Prompt as Administrator:**
    *   Open an elevated Command Prompt (`Run as administrator`).
    *   Navigate to your Sysinternals Suite directory: `cd C:\Sysinternals`.

2.  **Use Streams to Check the File:**
    *   Execute the `streams` command, pointing it to the file you created in the setup:
        ```bash
        streams C:\Temp\ads_test.txt
        ```
    *   Press Enter.

3.  **Analyze the Output:**
    *   The `streams` utility will report any ADS associated with the specified file.
    *   You should see your `hidden_stream` listed, along with its size.

4.  **Check a System File for ADS (Optional):**
    *   You can also check common system files (e.g., those downloaded from the internet) that Windows might have an ADS on for security zone information. For example, download a file from the internet (e.g., a PDF), then run `streams <path_to_downloaded_file>`. You will likely see a `:Zone.Identifier:$DATA` stream.

5.  **Remove the ADS (Optional):**
    *   To remove the detected ADS, you can use the `-d` switch:
        ```bash
        streams -d C:\Temp\ads_test.txt
        ```
    *   Run `streams C:\Temp\ads_test.txt` again to verify the ADS has been removed.

## Expected Output

```
(The output from the `streams` command will list any Alternate Data Streams found for the target file, including the one you created in setup.)

Example Output (after running `streams C:\Temp\ads_test.txt`):
Streams v1.60 - Reveal NTFS alternate streams.
Copyright (C) 1999-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

C:\Temp\ads_test.txt:
  Size      Name      Type
--------  --------  --------
      27  :hidden_stream:$DATA
```
![Expected output](images/exercise-08-output.png)

## Reflection Questions

1.  What is an Alternate Data Stream (ADS), and how does it allow data to be "hidden" within a file?
2.  How might malware leverage ADS to achieve persistence or evade detection by traditional antivirus software?
3.  Besides `streams`, what other tools or techniques could an analyst use to detect and investigate ADS in a suspicious file?

## Next Steps

*   [Exercise 09: Run as Another User](09-run-as-another-user.md)
*   Further reading: [Microsoft Docs: Streams](https://docs.microsoft.com/en-us/sysinternals/downloads/streams)

## Hints / Troubleshooting

*   If `streams` reports "No streams found", ensure you correctly created the ADS using the `echo` command. Double-check the path and syntax.
*   Remember that ADS are an NTFS file system feature. `streams` will not work on FAT32 or other file systems.
*   Windows Explorer and common file operations often do not show ADS, which is why `streams` is necessary.
