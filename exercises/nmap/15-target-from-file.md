---
Title: Exercise 15: Scan Targets from a File with Nmap (-iL)
Objective: Learn to provide Nmap with a list of target hosts from a file, enabling efficient scanning of many targets without typing each IP address or hostname on the command line.
Estimated Time: 10-20 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
*   A text editor (e.g., Notepad, VS Code)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Create a simple text file named `targets.txt` in your working directory.
3.  Populate `targets.txt` with a list of IP addresses or hostnames, one per line. For example:
    ```
    scanme.nmap.org
    192.168.1.1
    192.168.1.100-105
    ```
    (Replace with IPs/ranges relevant to your test environment and permissions).
---

## Steps

1.  **Create Your Target List File**
    *   Using a text editor, create a file named `targets.txt` (or any other name you prefer).
    *   Add your target IP addresses, hostnames, or network ranges to this file, with each entry on a new line.
    *   Example `targets.txt`:
        ```
        scanme.nmap.org
        192.168.1.1
        192.168.1.100-105
        # This is a comment, Nmap will ignore it
        ```

2.  **Perform a Scan Using the Target List**
    *   Open a terminal.
    *   Navigate to the directory where you saved `targets.txt`.
    *   Run Nmap using the `-iL` (input list) flag:
        ```bash
        nmap -F -iL targets.txt
        ```
    *   *Explanation:* The `-F` flag performs a "Fast Scan" (scans the 100 most common ports instead of 1000). The `-iL` flag instructs Nmap to read targets from the specified file.

3.  **Combine with Other Nmap Options**
    *   You can combine `-iL` with any other Nmap scanning options. For example, to perform a service version detection scan on all targets in your file and save the output:
        ```bash
        sudo nmap -sV -oA comprehensive_scan -iL targets.txt
        ```
    *   *Observation:* Nmap will process each target from the file sequentially (or in parallel depending on timing options) and output the results as usual, or save them to the specified output files.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 15:30 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
Not shown: 98 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
...
Nmap scan report for 192.168.1.1
Host is up (0.001s latency).
...
Nmap done: 7 IP addresses (X hosts up) scanned in Y.ZZ seconds
```


## Reflection Questions

1.  What are the primary benefits of using the `-iL` option when scanning a large number of hosts?
2.  How does the `-iL` option contribute to scripting and automation of Nmap scans?
3.  Can comments be included in the target list file, and how does Nmap handle them?

## Next Steps

*   [Exercise 16: Enumerate HTTP Directories with Nmap NSE](../nmap/16-http-enum-nse.md)
*   Further reading: [Nmap Target Specification](https://nmap.org/book/man-target-specification.html)

## Hints / Troubleshooting

*   **File Format:** Ensure the file contains one target per line. Nmap is generally robust and ignores blank lines and lines starting with `#`.
*   **Path:** If `targets.txt` is not in the same directory where you run Nmap, provide the full path to the file.
*   **Permissions:** For comprehensive scans, especially with service detection or OS fingerprinting, ensure you run Nmap with `sudo` or Administrator privileges.
*   **Large Lists:** For very large target lists, consider combining with timing templates (`-T4`) to manage scan speed and avoid overwhelming your network or the targets.
