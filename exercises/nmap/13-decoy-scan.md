---
Title: Exercise 13: Perform a Decoy Scan with Nmap (-D)
Objective: Learn to use Nmap's decoy scanning (`-D`) to make it appear as if multiple hosts are scanning the target simultaneously, obscuring your actual IP address among a list of decoys.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use a target on your local network (e.g., another VM or an unused IP). **Do NOT use real IP addresses of other organizations as decoys without permission.** You can use arbitrary non-existent IPs for demonstration, but be aware of the implications.
3.  Choose a few decoy IP addresses. These should ideally be actual hosts on the network that are up, or at least appear to be. You can also use `ME` as a decoy to place your own IP within the decoy list.
---

## Steps

1.  **Understand Decoy Scanning**
    *   The `-D` option allows you to specify a comma-separated list of decoy IP addresses, interspersed with `ME` (representing your actual IP address).
    *   Nmap will then send scan probes from your real IP and the specified decoys in a randomized order. This makes it difficult for an IDS/IPS to determine the true source of the scan.

2.  **Perform a Decoy Scan**
    *   Open a terminal.
    *   Choose some decoy IPs. For example, `192.168.1.100,192.168.1.101,ME,192.168.1.102`. Replace these with IPs relevant to your test environment.
    *   Run the following command:
        ```bash
        sudo nmap -sS -D 192.168.1.100,192.168.1.101,ME,192.168.1.102 [TargetIP]
        ```
    *   *Explanation:* Nmap will craft packets using various source IP addresses from the decoy list, making the target think multiple machines are scanning it.

3.  **Analyze the Scan (from Target's Perspective - Conceptual)**
    *   *Note:* To truly observe the effect of a decoy scan, you would need to monitor network traffic on the target machine using a packet analyzer like Wireshark.
    *   *Conceptual Observation:* If you were monitoring the target with Wireshark, you would see SYN packets originating from your specified decoy IPs, interspersed with packets from your actual IP, all directed at the target.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 14:30 EST
Nmap scan report for [TargetIP]
Host is up (0.0010s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
...
Nmap done: 1 IP address (1 host up) scanned in 10.50 seconds
```


## Reflection Questions

1.  How does a decoy scan help an attacker evade detection by network intrusion detection systems (IDS)?
2.  What are the limitations of decoy scanning, and why might it not always be effective against sophisticated defenses?
3.  What ethical considerations must be taken into account when using decoy IPs, especially if they belong to real, unconsenting third parties?

## Next Steps

*   [Exercise 14: Perform an Idle Scan (Zombie Scan) with Nmap (-sI)](../nmap/14-idle-scan.md)
*   Further reading: [Nmap Decoy Scan](https://nmap.org/book/man-bypass-firewalls.html#man-bypass-firewalls-decoy)

## Hints / Troubleshooting

*   **Permissions:** You MUST run this command with `sudo` or as Administrator.
*   **Decoy Selection:** Choose decoy IPs that are likely to be alive on the network to make the scan more believable. Using dead IPs might reveal the deception.
*   **Packet Capture:** The best way to understand decoy scanning is to use a packet sniffer (like Wireshark) on the target machine or an intermediate network device to see the source IPs of the incoming packets.
*   **IDS/IPS:** Advanced IDS/IPS systems can sometimes detect decoy scans by analyzing various metrics beyond just source IP, such as timing, TTL values, or TCP sequence numbers.
