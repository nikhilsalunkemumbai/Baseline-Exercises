---
Title: Exercise 07: Scan Port Ranges with Nmap (-p)
Objective: Learn to use Nmap's `-p` flag to specify exactly which ports or port ranges to scan, allowing for more targeted and efficient reconnaissance.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use `scanme.nmap.org` or a target on your local network.
---

## Steps

1.  **Scan a Specific Port**
    *   Open a terminal.
    *   Run the following command to scan only port 80:
        ```bash
        nmap -p 80 [TargetIP]
        ```

2.  **Scan a List of Ports**
    *   Scan ports 80, 443, and 22:
        ```bash
        nmap -p 80,443,22 [TargetIP]
        ```

3.  **Scan a Range of Ports**
    *   Scan ports 1 through 100:
        ```bash
        nmap -p 1-100 [TargetIP]
        ```

4.  **Scan All Ports**
    *   Scan all 65,535 possible TCP ports (this will take longer):
        ```bash
        nmap -p- [TargetIP]
        ```

5.  **Scan Ports by Name**
    *   Nmap can also scan ports by their service name (as defined in the `nmap-services` file):
        ```bash
        nmap -p http,https,ssh [TargetIP]
        ```

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 11:45 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).

PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 0.50 seconds
```
![Expected output](images/exercise-07-output.png)

## Reflection Questions

1.  Why is it often better to scan a specific set of ports rather than all 65,535?
2.  In what scenarios would you need to scan every single port on a target host?
3.  How does the `-p-` flag simplify the command for scanning the entire port range?

## Next Steps

*   [Exercise 08: Output Formats with Nmap (-oN, -oX, -oG)](../nmap/08-output-formats.md)
*   Further reading: [Nmap Port Specification and Scan Order](https://nmap.org/book/man-port-specification.html)

## Hints / Troubleshooting

*   **Time:** Scanning all ports (`-p-`) can take several minutes or more depending on your network speed and target responsiveness.
*   **Permissions:** For the best results, especially when scanning a wide range of ports, run Nmap as root/administrator.
*   **Output:** If you scan many ports and most are closed, Nmap will summarize them (e.g., "Not shown: 995 closed ports").
