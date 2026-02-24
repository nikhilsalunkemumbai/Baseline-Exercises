---
Title: Exercise 11: Adjust Timing Templates with Nmap (-T)
Objective: Learn to use Nmap's timing templates (`-T0` through `-T5`) to control the aggressiveness and speed of your scans, optimizing them for different network conditions and stealth requirements.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use `scanme.nmap.org` or a target on your local network that you have permission to scan.
---

## Steps

1.  **Understand Timing Templates**
    *   Nmap offers six timing templates (or "paranoid" to "insane"):
        *   `-T0 (Paranoid)`: Very slow, designed to evade IDS/IPS. Serializes scans.
        *   `-T1 (Sneaky)`: Very slow, similar to `-T0`, but slightly faster.
        *   `-T2 (Polite)`: Slows down to use less bandwidth and target machine resources.
        *   `-T3 (Normal)`: Default setting. Balances speed and stealth.
        *   `-T4 (Aggressive)`: Faster, assumes you are on a reasonably fast and reliable network. More likely to be detected.
        *   `-T5 (Insane)`: Very fast, can overwhelm targets or network devices. Not recommended for production.

2.  **Perform a Polite Scan (`-T2`)**
    *   Open a terminal.
    *   Run the following command:
        ```bash
        nmap -T2 [TargetIP]
        ```
    *   *Observation:* Note how long the scan takes compared to a default scan. You should notice it's significantly slower.

3.  **Perform an Aggressive Scan (`-T4`)**
    *   Run the following command:
        ```bash
        nmap -T4 [TargetIP]
        ```
    *   *Observation:* This scan should complete much faster than the `-T2` scan and potentially faster than the default scan, but might be noisier.

4.  **Perform a Very Slow Scan (`-T0` or `-T1`)**
    *   *Note:* These scans can take a very long time (minutes to hours). It's often impractical for a full port scan on a busy network.
    *   ```bash
        nmap -T0 -p 80,443 [TargetIP]
        ```
    *   *Observation:* Even for a few ports, the scan will be noticeably slower. Consider using `time` before the command (e.g., `time nmap -T0 -p 80,443 [TargetIP]`) to measure the duration.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 13:30 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
9929/tcp  open     nping-echo
31337/tcp open     Elite
Nmap done: 1 IP address (1 host up) scanned in X.XX seconds (varying based on -T template)
```


## Reflection Questions

1.  When would you choose to use a slower Nmap timing template (e.g., `-T0` or `-T1`) instead of the default or faster ones?
2.  What are the trade-offs between scan speed and stealth/detection when choosing a timing template?
3.  How can using an overly aggressive timing template (`-T5`) impact network stability or target system performance?

## Next Steps

*   [Exercise 12: Evade Firewalls with IP Fragmentation (-f)](../nmap/12-evade-firewalls-fragmentation.md)
*   Further reading: [Nmap Timing and Performance](https://nmap.org/book/man-performance.html)

## Hints / Troubleshooting

*   **Network Conditions:** The actual performance of each timing template will vary based on your network bandwidth, latency to the target, and the target's responsiveness.
*   **IDS/IPS:** Slower timing templates are designed to be less detectable by IDS/IPS, but no scan is truly undetectable.
*   **Default:** If you don't specify a timing template, Nmap uses `-T3 (Normal)`.
