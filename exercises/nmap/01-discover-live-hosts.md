---
Title: Exercise 01: Discover Live Hosts with Nmap Ping Scan
Objective: Learn to use Nmap to identify active (live) hosts on a network using a ping scan, which is the first step in network reconnaissance to avoid scanning dead IP addresses.
Estimated Time: 15-30 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Install Nmap on your machine.
2.  Identify your local network range (e.g., `192.168.1.0/24`). On Windows, use `ipconfig`. On Linux/macOS, use `ifconfig` or `ip a`.
3.  **Warning:** Only scan networks you own or have explicit permission to scan. For this exercise, use your local home network or a controlled lab environment.
---

## Steps

1.  **Perform a Simple Ping Scan**
    *   Open a terminal (Command Prompt or PowerShell on Windows).
    *   Run the following command, replacing `[YourNetworkRange]` with your actual network (e.g., `192.168.1.0/24`):
        ```bash
        nmap -sn [YourNetworkRange]
        ```
    *   *Explanation:* The `-sn` flag (formerly `-sP`) tells Nmap to perform a "No Port Scan". It only sends ICMP echo requests, TCP SYN to port 443, TCP ACK to port 80, and ICMP timestamp requests by default to determine if a host is up.

2.  **Analyze the Output**
    *   Review the list of hosts reported as "Host is up".
    *   *Observation:* Note the latency and the MAC addresses (if running with administrative/root privileges on a local subnet).

3.  **Scan a Specific IP Range or List**
    *   If you only want to scan a few specific IPs, you can list them or use a range:
        ```bash
        nmap -sn 192.168.1.1 192.168.1.5 192.168.1.10-20
        ```
    *   *Observation:* Observe how Nmap handles multiple targets efficiently.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 10:00 EST
Nmap scan report for 192.168.1.1
Host is up (0.0020s latency).
MAC Address: AA:BB:CC:DD:EE:FF (Router Manufacturer)
Nmap scan report for 192.168.1.5
Host is up (0.0050s latency).
MAC Address: 11:22:33:44:55:66 (Device Manufacturer)
Nmap scan report for 192.168.1.10
Host is up (0.012s latency).
MAC Address: 77:88:99:00:AA:BB (Your Computer)
Nmap done: 256 IP addresses (3 hosts up) scanned in 2.15 seconds
```


## Reflection Questions

1.  Why is a ping scan useful before performing a full port scan on a large network?
2.  If a host is actually up but does not respond to a ping scan, what might be the reason? (Hint: Think about firewalls).
3.  What is the difference between `-sn` and a default Nmap scan without any flags?

## Next Steps

*   [Exercise 02: Scan Common Ports](../nmap/02-scan-common-ports.md)
*   Further reading: [Nmap Host Discovery documentation](https://nmap.org/book/man-host-discovery.html)

## Hints / Troubleshooting

*   **Permissions:** On local networks, Nmap uses ARP requests if run as root/administrator, which is very reliable. If you are not seeing MAC addresses, try running with `sudo nmap -sn ...` on Linux/macOS or as Administrator on Windows.
*   **Firewalls:** Many modern systems block ICMP (ping) requests. If you know a host is up but Nmap says it's down, try adding `-Pn` to skip host discovery, but this is for port scanning, not ping scans.
*   **Virtual Machines:** If scanning between VMs, ensure they are on the same virtual network (e.g., "Bridged" or "Host-Only").
