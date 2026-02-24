---
Title: Exercise 05: Aggressive Scan with Nmap (-A)
Objective: Learn to use Nmap's "Aggressive Scan" mode to combine multiple powerful scanning techniques (OS detection, service version detection, script scanning, and traceroute) into a single command for comprehensive target assessment.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use `scanme.nmap.org` (a test server provided by the Nmap project) or a target on your local network that you have permission to scan.
---

## Steps

1.  **Perform an Aggressive Scan**
    *   Open a terminal.
    *   Run the following command, replacing `[TargetIP]` with your target (e.g., `scanme.nmap.org`):
        ```bash
        sudo nmap -A [TargetIP]
        ```
    *   *Note:* Aggressive scan requires administrative/root privileges on all operating systems.
    *   *Explanation:* The `-A` flag enables OS detection (`-O`), service version detection (`-sV`), script scanning (`-sC`), and traceroute (`--traceroute`). This is a very active and comprehensive scan mode.

2.  **Analyze the Output**
    *   Review the output, which will now include:
        *   Port status and service versions (from `-sV`).
        *   OS details and confidence level (from `-O`).
        *   Results from various default NSE (Nmap Scripting Engine) scripts (from `-sC`).
        *   Traceroute information showing the network path to the target.

3.  **Refine Detection (Optional)**
    *   If the scan takes too long, try adding a timing template (e.g., `-T4`) to speed it up.
        ```bash
        sudo nmap -A -T4 [TargetIP]
        ```
    *   *Observation:* Observe if Nmap provides the same level of detail in less time.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 11:00 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE      VERSION
22/tcp    open     ssh          OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 11:22:33:44:55:66:77:88:99:00 (DSA)
|_  2048 AA:BB:CC:DD:EE:FF:11:22:33:44 (RSA)
80/tcp    open     http         Apache httpd 2.4.7 ((Ubuntu))
|_http-title: Go ahead and ScanMe!
9929/tcp  open     nping-echo   Nping echo
31337/tcp open     Elite

Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 11 hops

TRACEROUTE (using port 443/tcp)
HOP RTT      ADDRESS
...
11  50.00 ms scanme.nmap.org (45.33.32.156)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.40 seconds
```
![Expected output](images/exercise-05-output.png)

## Reflection Questions

1.  How does the `-A` flag simplify the scanning process compared to running multiple individual Nmap commands?
2.  What are the disadvantages of using an aggressive scan in a sensitive or production environment?
3.  How can the script output (from `-sC`) provide additional security insights beyond just service versions?

## Next Steps

*   [Exercise 06: UDP Port Scan with Nmap (-sU)](../nmap/06-udp-port-scan.md)
*   Further reading: [Nmap Aggressive Scan](https://nmap.org/book/man-briefoptions.html#man-briefoptions-A)

## Hints / Troubleshooting

*   **Permissions:** You MUST run this command with `sudo` or as Administrator.
*   **Target Not Responding:** If you see "0 hosts up", try adding `-Pn` to assume the host is up, even if it doesn't respond to ping probes.
*   **Intrusion Detection Systems (IDS):** Aggressive scanning is very active and can be easily detected by IDS/IPS. Use it carefully in sensitive environments.
*   **Time:** Aggressive scanning takes significantly longer than a basic scan. Use timing templates (e.g., `-T4`) to speed things up if appropriate.
