---
Title: Exercise 02: Filter by IP
Objective: Learn to filter a Wireshark capture to display only traffic to and from a specific IP address.
Estimated Time: 20-30 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. (Optional) Identify an IP address on your local network that you want to monitor (e.g., your router's IP, another device's IP, or a public IP like 8.8.8.8 for Google DNS).
---

## Steps

1.  **Start a New Capture (or Open Existing PCAP):**
    *   **Option A (Live Capture):** Launch Wireshark, select your active network interface, and click "Start capturing packets". Allow it to capture traffic for a few minutes while you perform some network activity (e.g., browse a website, ping a server).
    *   **Option B (Existing PCAP):** Go to `File > Open` and select a pre-captured PCAP file.

2.  **Identify a Target IP Address:**
    *   If doing a live capture, let it run for a moment. Observe the "Source" and "Destination" columns in the packet list pane. Pick an IP address you want to investigate (e.g., your default gateway, a DNS server like 8.8.8.8, or a public website's IP).
    *   If using an existing PCAP, scroll through the packets and identify an IP address of interest.

3.  **Apply the IP Address Display Filter:**
    *   In the Wireshark display filter bar, type `ip.addr == X.X.X.X` (replace `X.X.X.X` with your chosen IP address).
    *   Press Enter to apply the filter.

4.  **Inspect Filtered Traffic:**
    *   Observe the packet list pane. All displayed packets should now have your specified IP address as either the "Source" or "Destination".
    *   Examine the "Protocol" and "Info" columns to understand the types of traffic associated with this IP.

## Expected Output

```
(Expected output will show only packets where the specified IP address is either the source or destination. The example below uses 8.8.8.8 as the target IP.)

Example of Filtered Packet List:
No.     Time        Source          Destination     Protocol Length Info
5       0.000123    192.168.1.100   8.8.8.8         DNS      75     Standard query A www.google.com
6       0.000345    8.8.8.8         192.168.1.100   DNS      75     Standard query response A 142.250.72.36
12      0.123456    192.168.1.100   8.8.8.8         TCP      66     55482 → 53 [SYN] Seq=0 Win=64240 Len=0 MSS=1460 SACK_PERM=1 WS=256
13      0.123890    8.8.8.8         192.168.1.100   TCP      66     53 → 55482 [SYN, ACK] Seq=0 Ack=1 Win=29200 Len=0 MSS=1460 SACK_PERM=1 WS=128
... (all packets will involve 8.8.8.8 as either source or destination)
```


## Reflection Questions

1.  What is the difference between `ip.addr == X.X.X.X` and `ip.src == X.X.X.X` (or `ip.dst == X.X.X.X`)? When would you use each?
2.  How can filtering by IP address help in troubleshooting network connectivity issues?
3.  Why might you sometimes still see traffic from other IPs even after applying an IP filter (e.g., ARP requests)?

## Next Steps

*   [Exercise 03: Reconstruct FTP File](03-reconstruct-ftp-file.md)
*   Further reading: [Wireshark Display Filter Cheatsheet](https://www.wireshark.org/docs/wsug_html_chunked/ChBuildDisplayFilter.html)

## Hints / Troubleshooting

*   If no packets are displayed after applying the filter, ensure the IP address is correct and that traffic involving that IP occurred during the capture.
*   Check for typos in the filter expression. Wireshark will often indicate invalid filter syntax with a red background in the filter bar.
*   For local network IPs, ensure your capture interface is correctly selected.
