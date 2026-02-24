---
Title: Exercise 12: Evade Firewalls with IP Fragmentation (-f)
Objective: Learn to use Nmap's IP fragmentation option to break scan probes into tiny IP fragments, potentially bypassing simple packet filtering firewalls that might only inspect the first fragment.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, ideally, you would have a simple firewall rule configured to drop packets that aren't fragmented. You can test against `scanme.nmap.org` as well.
---

## Steps

1.  **Perform a Standard Scan (Baseline)**
    *   Open a terminal.
    *   Run a standard SYN scan against your target:
        ```bash
        sudo nmap -sS -p 80,443 [TargetIP]
        ```
    *   *Observation:* Note the results. This is your baseline.

2.  **Perform a Fragmented Scan**
    *   Now, add the `-f` flag to fragment the IP packets:
        ```bash
        sudo nmap -sS -f -p 80,443 [TargetIP]
        ```
    *   *Explanation:* The `-f` option causes the requested SYN (or other scan type) probes to be broken into multiple smaller IP packets. Nmap divides the IP header over several packets, making it harder for basic packet filters to reassemble and detect the scan type. The `-f` flag will split packets into 8-byte fragments (minus the IP header). Using `-f` twice (`-f -f` or `-ff`) will fragment them into 16-byte chunks.

3.  **Analyze the Output**
    *   Compare the output of the fragmented scan with your baseline scan.
    *   *Observation:* Ideally, if there was a firewall designed to detect unfragmented packets, the fragmented scan might reveal more open ports or bypass detection. On `scanme.nmap.org`, the results might be identical as it's designed to be scanned robustly.

4.  **Try Double Fragmentation (Optional)**
    *   ```bash
        sudo nmap -sS -ff -p 80,443 [TargetIP]
        ```
    *   *Observation:* Does using double fragmentation (`-ff`) change the results or timing compared to single fragmentation (`-f`)?

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 14:00 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
PORT    STATE SERVICE
80/tcp  open  http
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 1.20 seconds
```


## Reflection Questions

1.  How does IP fragmentation work as a firewall evasion technique?
2.  What types of firewalls or network devices are most susceptible to being bypassed by IP fragmentation?
3.  What are the disadvantages of using IP fragmentation during a scan, from both an attacker's and a defender's perspective?

## Next Steps

*   [Exercise 13: Perform a Decoy Scan with Nmap (-D)](../nmap/13-decoy-scan.md)
*   Further reading: [Nmap Fragmentation Options](https://nmap.org/book/man-bypass-firewalls.html#man-bypass-firewalls-fragment)

## Hints / Troubleshooting

*   **Permissions:** You MUST run this command with `sudo` or as Administrator.
*   **Effectiveness:** IP fragmentation is less effective against modern, stateful firewalls or IDS/IPS systems that can reassemble fragmented packets before applying rules.
*   **Network Performance:** Fragmentation can increase network overhead and potentially slow down scans or cause issues on unstable networks.
*   **Verification:** To truly see fragmentation in action, you'd need a packet analyzer like Wireshark on the network segment between Nmap and the target.
