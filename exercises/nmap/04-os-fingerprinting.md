---
Title: Exercise 04: OS Fingerprinting with Nmap (-O)
Objective: Learn to use Nmap to identify the underlying operating system (OS) of a target host by analyzing the TCP/IP stack behavior in response to specific probes.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use `scanme.nmap.org` (a test server provided by the Nmap project) or a target on your local network that you have permission to scan.
---

## Steps

1.  **Perform an OS Fingerprinting Scan**
    *   Open a terminal.
    *   Run the following command, replacing `[TargetIP]` with your target (e.g., `scanme.nmap.org`):
        ```bash
        sudo nmap -O [TargetIP]
        ```
    *   *Note:* OS Fingerprinting requires administrative/root privileges on all operating systems (including Windows, where you'll run as Administrator).
    *   *Explanation:* The `-O` flag enables OS detection. Nmap sends a series of TCP and UDP packets to the remote host and examines practically every bit in the responses. After performing dozens of tests, Nmap compares the results to its `nmap-os-db` database of thousands of known OS fingerprints and prints the OS details if there is a match.

2.  **Analyze the Output**
    *   Review the "Device type", "Running", "OS CPE", and "OS details" lines in the scan report.
    *   *Observation:* Note the confidence percentage (e.g., `OS details: Linux 3.2 - 4.9`).

3.  **Refine Detection (Optional)**
    *   If Nmap is unable to determine the OS, try `--osscan-guess` or `--osscan-limit`. `--osscan-guess` makes Nmap guess more aggressively.
        ```bash
        sudo nmap -O --osscan-guess [TargetIP]
        ```
    *   *Observation:* Observe if Nmap provides a more specific guess.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 10:45 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
9929/tcp  open     nping-echo
31337/tcp open     Elite

Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 11 hops

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.30 seconds
```


## Reflection Questions

1.  Why does OS fingerprinting require at least one open and one closed TCP port for the best results?
2.  How could knowing the OS of a target system help an incident responder during a forensic investigation?
3.  Why does OS fingerprinting often require root/administrative privileges, unlike a basic Nmap scan?

## Next Steps

*   [Exercise 05: Aggressive Scan with Nmap (-A)](../nmap/05-aggressive-scan.md)
*   Further reading: [Nmap OS Detection](https://nmap.org/book/osdetect.html)

## Hints / Troubleshooting

*   **Permissions:** You MUST run this command with `sudo` or as Administrator.
*   **Target Selection:** Some hosts (especially those behind firewalls or load balancers) may not provide enough information for accurate OS fingerprinting.
*   **Multiple OS Matches:** If Nmap shows multiple possible OS matches, it's because the target host's stack matches several entries in its database. This is common for similar Linux kernel versions.
*   **Time:** OS fingerprinting adds several seconds to your scan as it performs multiple probes.
