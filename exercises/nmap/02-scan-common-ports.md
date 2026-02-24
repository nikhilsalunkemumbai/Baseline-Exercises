---
Title: Exercise 02: Scan Common Ports with Nmap Default Scan
Objective: Learn to use Nmap to scan a target host and identify which of the 1,000 most common TCP ports are open, providing an initial look at the services running on a system.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use `scanme.nmap.org` (a test server provided by the Nmap project) or a target on your local network that you have permission to scan.
---

## Steps

1.  **Perform a Default Scan**
    *   Open a terminal.
    *   Run the following command, replacing `[TargetIP]` with your target (e.g., `scanme.nmap.org`):
        ```bash
        nmap [TargetIP]
        ```
    *   *Explanation:* By default, Nmap scans the top 1,000 most common TCP ports for each host. It uses a "SYN Stealth Scan" (`-sS`) if run with administrative/root privileges, or a "TCP Connect Scan" (`-sT`) otherwise.

2.  **Analyze the Output**
    *   Review the table of ports.
    *   *Observation:* Note the "PORT", "STATE", and "SERVICE" columns. "Open" indicates a service is listening, "Closed" means no service is listening, and "Filtered" means a firewall is likely blocking the scan.

3.  **Scan for Specific Port States**
    *   Observe the "Open" ports and their associated service names (e.g., `80/tcp open http`, `22/tcp open ssh`).

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 10:15 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
9929/tcp  open     nping-echo
31337/tcp open     Elite
Nmap done: 1 IP address (1 host up) scanned in 10.50 seconds
```
![Expected output](images/exercise-02-output.png)

## Reflection Questions

1.  What is the difference between an "Open" port and a "Filtered" port in Nmap's output?
2.  If a port is "Closed", does that mean the host is down?
3.  Why does Nmap only scan 1,000 ports by default instead of all 65,535 possible TCP ports?

## Next Steps

*   [Exercise 03: Service Version Detection](../nmap/03-service-version-detection.md)
*   Further reading: [Nmap Port Scanning Basics](https://nmap.org/book/man-port-scanning-basics.html)

## Hints / Troubleshooting

*   **Permissions:** Running as root/administrator (`sudo nmap ...`) is generally more powerful and accurate, especially for stealth scanning (`-sS`).
*   **Target Not Responding:** If you see "0 hosts up", try adding `-Pn` to assume the host is up, even if it doesn't respond to ping probes.
*   **Time:** A default scan on 1,000 ports is usually very fast. If it takes a long time, check your network connection or try a timing template like `-T4`.
