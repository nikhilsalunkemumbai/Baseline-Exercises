---
Title: Exercise 06: UDP Port Scan with Nmap (-sU)
Objective: Learn to use Nmap to scan a target host for open UDP ports, which are often overlooked but can host critical services like DNS, DHCP, and SNMP.
Estimated Time: 30-45 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use a target on your local network that you have permission to scan (e.g., a router or another VM).
---

## Steps

1.  **Perform a UDP Port Scan**
    *   Open a terminal.
    *   Run the following command, replacing `[TargetIP]` with your target:
        ```bash
        sudo nmap -sU [TargetIP]
        ```
    *   *Note:* UDP scanning requires administrative/root privileges on all operating systems.
    *   *Explanation:* The `-sU` flag enables UDP scanning. UDP is a connectionless protocol, so Nmap cannot rely on a simple three-way handshake like it does with TCP. Instead, it sends UDP packets and waits for a response (which might be another UDP packet, or an ICMP "Port Unreachable" message if the port is closed).

2.  **Analyze the Output**
    *   Review the table of ports.
    *   *Observation:* Note the "STATE" column. "Open" indicates a service responded. "Open|Filtered" means Nmap sent a probe but received no response, so it cannot distinguish between an open port and one blocked by a firewall. "Closed" means an ICMP "Port Unreachable" was received.

3.  **Scan for Specific UDP Ports**
    *   If you only want to scan specific common UDP ports (e.g., 53 for DNS, 67 for DHCP, 161 for SNMP):
        ```bash
        sudo nmap -sU -p 53,67,161 [TargetIP]
        ```
    *   *Observation:* Observe if Nmap identifies any specific services on these ports.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 11:30 EST
Nmap scan report for 192.168.1.1 (Router)
Host is up (0.0020s latency).
Not shown: 995 closed udp ports (port-unreach)
PORT      STATE          SERVICE
53/udp    open           domain
67/udp    open|filtered  dhcps
161/udp   open|filtered  snmp
1900/udp  open|filtered  upnp

Nmap done: 1 IP address (1 host up) scanned in 25.60 seconds
```


## Reflection Questions

1.  Why is UDP scanning generally much slower and less reliable than TCP scanning?
2.  What is the significance of the "Open|Filtered" state in Nmap's UDP scan results?
3.  How can an attacker exploit open UDP services like DNS or SNMP on a target network?

## Next Steps

*   [Exercise 07: Scan Port Ranges with Nmap (-p)](../nmap/07-scan-port-ranges.md)
*   Further reading: [Nmap UDP Scan](https://nmap.org/book/man-port-scanning-techniques.html#man-sU)

## Hints / Troubleshooting

*   **Permissions:** You MUST run this command with `sudo` or as Administrator.
*   **Time:** UDP scanning is inherently slow because Nmap must wait for timeouts if no response is received. Use `-p` to limit the number of ports scanned to speed things up.
*   **Accuracy:** UDP scans can be less accurate due to network congestion or firewalls silently dropping packets.
*   **Service Detection:** Adding `-sV` to a UDP scan can help confirm "Open|Filtered" ports by sending service-specific probes.
