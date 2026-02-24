---
Title: Exercise 03: Service Version Detection with Nmap (-sV)
Objective: Learn to use Nmap to determine what specific service version and application name is running on a target host's open ports, which is critical for identifying potential vulnerabilities.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use `scanme.nmap.org` (a test server provided by the Nmap project) or a target on your local network that you have permission to scan.
---

## Steps

1.  **Perform a Service Version Scan**
    *   Open a terminal.
    *   Run the following command, replacing `[TargetIP]` with your target (e.g., `scanme.nmap.org`):
        ```bash
        nmap -sV [TargetIP]
        ```
    *   *Explanation:* The `-sV` flag (Service/Version detection) instructs Nmap to probe open ports to determine what software and version are actually running. This goes beyond the default service mapping (e.g., port 80 = "http").

2.  **Analyze the Output**
    *   Review the table of ports.
    *   *Observation:* Note the "VERSION" column. It should provide specific details like "Apache httpd 2.4.7", "OpenSSH 6.6.1p1", etc.

3.  **Adjust Version Intensity (Optional)**
    *   Nmap uses multiple probes to identify services. You can increase the "intensity" with `--version-intensity 0-9`. The default is 7. Higher values are more accurate but take longer.
        ```bash
        nmap -sV --version-intensity 9 [TargetIP]
        ```
    *   *Observation:* Observe if any additional details are revealed with higher intensity.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 10:30 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE      VERSION
22/tcp    open     ssh          OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
80/tcp    open     http         Apache httpd 2.4.7 ((Ubuntu))
9929/tcp  open     nping-echo   Nping echo
31337/tcp open     Elite        (No version detected)
Nmap done: 1 IP address (1 host up) scanned in 15.20 seconds
```


## Reflection Questions

1.  How does service version detection help an incident responder or security auditor beyond just knowing which ports are open?
2.  If Nmap cannot determine the specific version, what does that imply about the service or Nmap's capabilities?
3.  Why does a scan with `-sV` take significantly longer than a default Nmap scan?

## Next Steps

*   [Exercise 04: OS Fingerprinting with Nmap (-O)](../nmap/04-os-fingerprinting.md)
*   Further reading: [Nmap Service and Version Detection](https://nmap.org/book/vscan.html)

## Hints / Troubleshooting

*   **Permissions:** For the best results, run Nmap as root/administrator (`sudo nmap ...`).
*   **Intrusion Detection Systems (IDS):** Service version detection is very active and can be easily detected by IDS/IPS. Use it carefully in sensitive environments.
*   **Time:** Service detection can be slow if there are many open ports. Use timing templates (e.g., `-T4`) to speed things up if appropriate.
