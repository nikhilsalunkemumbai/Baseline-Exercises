---
Title: Exercise 14: Perform an Idle Scan (Zombie Scan) with Nmap (-sI)
Objective: Learn to use Nmap's advanced Idle Scan technique to perform a port scan against a target while completely spoofing your source IP address, making the scan appear to originate from an innocent third-party "zombie" host.
Estimated Time: 30-60 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
*   A suitable "zombie" host.
Setup Instructions:
1.  **Nmap Installation:** Ensure Nmap is installed and accessible from your terminal.
2.  **Target Selection:** Identify a target IP address or hostname. For this exercise, use a target on your local network that you have permission to scan.
3.  **Zombie Host:** This is the critical part. An ideal zombie host must meet specific criteria:
    *   **Idle:** It must be relatively inactive on the network.
    *   **IP ID Sequence:** It must have a predictable and incrementing IP ID sequence. Most modern OS (Linux, Windows) use random IP ID sequences, making them unsuitable. Older or specific embedded systems, printers, or some IoT devices might be good candidates.
    *   **No Firewall:** It should not have a firewall blocking incoming traffic, as Nmap needs to interact with it.
    *   *Finding a Zombie:* This is often the hardest part. You might need to set up an older VM (e.g., Windows XP or certain Linux distros with specific kernel configurations) or use a device known to have predictable IP ID sequences on your test network.
    *   **Warning:** Do NOT use unknown hosts on public networks as zombies. This is an advanced and potentially illegal technique if used without authorization. For this exercise, use a device you control or a designated test system.
---

## Steps

1.  **Verify Zombie Host Suitability**
    *   Open a terminal.
    *   To check if a potential zombie has an incrementing IP ID sequence, you can try sending a few packets and observing the IP ID.
    *   ```bash
        nmap -sS -p80 [ZombieIP]
        ```
    *   *Explanation:* After this scan, Nmap will report the IP ID sequence type for the zombie. You are looking for "Incrementing" or "Broken little endian incrementing". "Random" or "All zero" means it's not a suitable zombie.

2.  **Perform an Idle Scan**
    *   Once you have identified a suitable zombie IP, run the idle scan:
        ```bash
        sudo nmap -sI [ZombieIP] [TargetIP]
        ```
    *   *Explanation:* Nmap communicates only with the zombie host and the target communicates with the zombie. Your IP never directly sends probes to the target. Nmap sends SYN packets to the zombie to get its IP ID, then sends a spoofed SYN packet (with the zombie's IP as source) to the target. Based on how the zombie's IP ID changes after the spoofed packet, Nmap infers the target's port state.

3.  **Analyze the Output**
    *   The output will show the open/closed/filtered ports on the *target*, but the scan was seemingly performed by the *zombie*.
    *   *Observation:* Note that your own IP address is not mentioned as the source of the scan.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 15:00 EST
Idle scan using zombie 192.168.1.5 (192.168.1.5:80)
Nmap scan report for 192.168.1.10 (Target Host)
Host is up (0.0010s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
...
Nmap done: 1 IP address (1 host up) scanned in 20.15 seconds
```


## Reflection Questions

1.  How does the Idle Scan ("Zombie Scan") technique achieve complete anonymity for the scanner?
2.  What specific characteristics must a host possess to be a viable "zombie" for an idle scan, and why are most modern operating systems unsuitable?
3.  What are the major challenges and ethical considerations involved in performing an idle scan?

## Next Steps

*   [Exercise 15: Scan Targets from a File with Nmap (-iL)](../nmap/15-target-from-file.md)
*   Further reading: [Nmap Idle Scan](https://nmap.org/book/idlescan.html)

## Hints / Troubleshooting

*   **Permissions:** You MUST run this command with `sudo` or as Administrator.
*   **Zombie Selection is Key:** The success of an idle scan depends entirely on finding a suitable zombie host. If Nmap reports "IP ID sequence is random," that host cannot be used.
*   **Time:** Idle scans can be very slow due to the indirect nature of probing.
*   **Packet Capture:** Use Wireshark on the zombie and target hosts to observe the packet flow and understand how the idle scan works.
*   **Firewalls:** Firewalls blocking the zombie from reaching the target, or the scanner from reaching the zombie, will interfere with the scan.
