---
Title: Exercise 04: Detect Port Scan
Objective: Identify and analyze a port scan attempt within a Wireshark capture file by observing numerous sequential connection attempts (SYN packets without corresponding ACKs).
Estimated Time: 45-60 minutes
Tools Needed: Wireshark, PCAP file containing a simulated port scan.
Setup Instructions: 
1. Install Wireshark on your system.
2. Obtain a PCAP file with a simulated port scan. You can:
    *   **Generate your own (Recommended in a Lab Environment):**
        *   Set up a controlled lab environment with a scanning machine (e.g., Kali Linux) and a target machine (e.g., Metasploitable, a Windows VM).
        *   On the machine you're running Wireshark on (or a machine that can mirror traffic from the target), start a Wireshark capture.
        *   From the scanning machine, perform a SYN scan (stealth scan) against the target machine. For example: `nmap -sS -p 1-100 192.168.1.X` (replace `192.168.1.X` with the target IP and `-p 1-100` to scan ports 1-100).
        *   Stop the Wireshark capture after Nmap completes.
    *   **Use a sample:** Search for publicly available PCAP samples that include port scan traffic. *Be cautious with unknown sources.*
---

## Steps

1.  **Open the PCAP File in Wireshark:**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file containing the port scan traffic.

2.  **Filter for SYN Packets:**
    *   In the display filter bar, type `tcp.flags.syn == 1` and press Enter. This filter displays all TCP SYN packets, which are the initial packets sent to initiate a TCP connection. These are characteristic of SYN/stealth scans.

3.  **Analyze SYN Packet Patterns:**
    *   Observe the "Info" column and "Destination Port" for the filtered packets.
    *   Look for a single source IP sending SYN packets to many different destination ports on a target IP address. This indicates a horizontal port scan (scanning multiple ports on one host).
    *   Alternatively, look for a single source IP sending SYN packets to the same port across many different target IP addresses. This could indicate a vertical scan (scanning one port on many hosts).

4.  **Identify Missing ACK Responses (for Stealth Scans):**
    *   To specifically look for stealth (SYN) scans, where the scanner does *not* complete the 3-way handshake, filter for SYN packets and then look for the absence of corresponding `SYN, ACK` and `ACK` packets.
    *   A common pattern for SYN scan detection is to filter for `tcp.flags.syn == 1 and tcp.flags.ack == 0`. This highlights packets that are initiating a connection but are not acknowledgments to a previous SYN.
    *   You can also examine the `tcp.analysis.flags` column (if added) or `Analyze > Expert Information` for retransmissions or missing acknowledgements.

5.  **Look for Sequential Port Attempts (Optional):**
    *   Sort the packets by "Time" or "Destination Port".
    *   Observe if the destination ports are incrementing sequentially or appear in a rapid, ordered fashion from a single source to a single destination.

6.  **Use Conversation Statistics (Optional):**
    *   Go to `Statistics > Conversations`.
    *   Select the "TCP" tab.
    *   Sort by "Packets" or "Bytes" and look for conversations with a high number of SYN packets without a balanced exchange of ACK or FIN packets. This can highlight scan activity.

## Expected Output

```
(Expected output will show numerous SYN packets from a scanning IP to various ports on a target IP. Often, these SYN packets will not be followed by a full 3-way handshake from the scanner's perspective in a stealth scan.)

Example of Filtered Packet List (tcp.flags.syn == 1):
No.     Time        Source          Destination     Protocol Length Info
10      0.000100    192.168.1.10    192.168.1.100   TCP      74     50000 > 22 [SYN] Seq=0 Win=65535 Len=0 MSS=1460 WS=256 SACK_PERM=1
11      0.000150    192.168.1.10    192.168.1.100   TCP      74     50001 > 80 [SYN] Seq=0 Win=65535 Len=0 MSS=1460 WS=256 SACK_PERM=1
12      0.000200    192.168.1.10    192.168.1.100   TCP      74     50002 > 443 [SYN] Seq=0 Win=65535 Len=0 MSS=1460 WS=256 SACK_PERM=1
... (many more SYN packets to different destination ports)

Example of Expert Information indicating issues (Analyze > Expert Information):
Severity Level: Note
Group: TCP
Summary: TCP Port numbers reused
...
Summary: TCP Retransmission
...
```


## Reflection Questions

1.  How does a SYN scan (stealth scan) attempt to evade detection compared to a full TCP connect scan?
2.  What other Wireshark features or filters could help differentiate between legitimate traffic and a port scan?
3.  Why might it be difficult to detect a very slow or highly distributed port scan using just Wireshark?

## Next Steps

*   [Exercise 05: Extract DNS Queries](05-extract-dns-queries.md)
*   Further reading: [Wireshark Wiki: TCP Port Scan Detection](https://wiki.wireshark.org/Port_Scanning)

## Hints / Troubleshooting

*   If you're not seeing many SYN packets, ensure your Nmap scan type was indeed a SYN scan (`-sS`).
*   Verify that your Wireshark capture was running on an interface that could see the scan traffic (e.g., on the target machine, or a port-mirrored interface).
*   Sometimes, firewalls or intrusion detection systems can block or alter scan traffic, making it harder to analyze.
