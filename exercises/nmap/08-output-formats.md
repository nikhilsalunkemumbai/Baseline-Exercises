---
Title: Exercise 08: Output Formats with Nmap (-oN, -oX, -oG)
Objective: Learn to save Nmap scan results into various file formats (Normal, XML, and Greppable) for documentation, reporting, and further automated analysis.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use `scanme.nmap.org` or a target on your local network.
---

## Steps

1.  **Save in Normal Format (-oN)**
    *   Open a terminal.
    *   Run the following command, replacing `[TargetIP]` and `[OutputFile]` (e.g., `scan_normal.txt`):
        ```bash
        nmap -oN scan_normal.txt [TargetIP]
        ```
    *   *Explanation:* The `-oN` flag saves the output exactly as it appears on your screen in a standard text file.

2.  **Save in XML Format (-oX)**
    *   Run the following command to save in XML format:
        ```bash
        nmap -oX scan_xml.xml [TargetIP]
        ```
    *   *Explanation:* The `-oX` flag saves the output in a structured XML format, which is ideal for importing into other tools (like SIEMs, reporting tools, or custom scripts).

3.  **Save in Greppable Format (-oG)**
    *   Run the following command to save in a format easy to parse with `grep`:
        ```bash
        nmap -oG scan_grep.txt [TargetIP]
        ```
    *   *Explanation:* The `-oG` flag saves the output in a "one host per line" format, making it very easy to search for specific open ports or services using command-line tools like `grep`, `awk`, or `cut`.

4.  **Save in All Formats at Once (-oA)**
    *   Run the following command to save in all three formats (Normal, XML, and Greppable) with the same base filename:
        ```bash
        nmap -oA scan_all [TargetIP]
        ```
    *   *Observation:* This will create three files: `scan_all.nmap`, `scan_all.xml`, and `scan_all.gnmap`.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 12:00 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
...
Nmap done: 1 IP address (1 host up) scanned in 10.50 seconds

(Check your directory for the created files: scan_normal.txt, scan_xml.xml, scan_grep.txt, scan_all.nmap, scan_all.xml, scan_all.gnmap)
```
![Expected output](images/exercise-08-output.png)

## Reflection Questions

1.  Which output format is best for manual review and documentation?
2.  In what scenarios would the XML format be more useful than the Normal text format?
3.  How can you use the Greppable output format with the `grep` command to find all hosts that have port 80 open?

## Next Steps

*   [Exercise 09: Nmap Scripting Engine (NSE) Basics (--script)](../nmap/09-nmap-scripting-engine-nse.md)
*   Further reading: [Nmap Output Formats](https://nmap.org/book/man-output.html)

## Hints / Troubleshooting

*   **File Overwriting:** Nmap will overwrite existing files with the same name. Be careful when choosing your output filenames.
*   **Path:** If you don't specify a path, the files will be saved in your current working directory.
*   **Permissions:** Ensure you have write permissions to the directory where you are saving the files.
