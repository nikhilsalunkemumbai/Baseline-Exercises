---
Title: Exercise 12: Top Talkers
Objective: Identify the "top talkers" on a network segment by using Wireshark's "Statistics > Endpoints" feature to view which IP addresses sent or received the most packets and/or bytes.
Estimated Time: 15-25 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Generate varied network traffic for a few minutes (e.g., browse to several websites, download a small file, stream some audio/video, or run a network-intensive application). Stop the capture.
4. **Option B (Existing PCAP):** Use a pre-captured PCAP file that contains a decent amount of network traffic.
---

## Steps

1.  **Open the PCAP File in Wireshark (or use Live Capture):**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file, or start and stop a live capture as described in Setup.

2.  **Open the Endpoints Statistics Window:**
    *   Go to `Statistics > Endpoints`. A new window titled "Wireshark Endpoints" will open.

3.  **Analyze IP Endpoints:**
    *   In the "Wireshark Endpoints" window, select the "IPv4" tab (or "IPv6" if you are focusing on IPv6 traffic).
    *   Observe the columns: "Address", "Packets", "Bytes", "Tx Packets", "Rx Packets", "Tx Bytes", "Rx Bytes", "Tx Util", "Rx Util".
    *   **Sort by "Packets" or "Bytes":** Click on the "Packets" column header to sort the list in descending order. The IP address at the top is the one that has sent/received the most packets. Do the same for the "Bytes" column.
    *   Identify the IP addresses that appear at the top of the sorted lists. These are your "top talkers" or "top listeners" depending on the sort order.

4.  **Use "Limit to display filter" (Optional):**
    *   If you have a display filter applied in the main Wireshark window (e.g., `http` or `tcp.port == 80`), the "Endpoints" window will, by default, show statistics for *all* captured traffic.
    *   To see statistics only for the currently displayed traffic, check the "Limit to display filter" box at the bottom of the "Wireshark Endpoints" window. This can help narrow down top talkers for specific types of traffic.

5.  **Apply Filter from Endpoints (Optional):**
    *   Right-click on an IP address in the "Endpoints" list.
    *   Select `Apply as Filter > Selected > A and B` (or `Selected > A only`, `Selected > B only`) to quickly filter the main Wireshark display for traffic involving that specific IP.

## Expected Output

```
(The "Wireshark Endpoints" window, particularly the IPv4 tab, will show a list of IP addresses. Sorting this list by "Packets" or "Bytes" will reveal the top-contributing IPs.)

Example of "Wireshark Endpoints" window (IPv4 tab, sorted by Packets):
Address           Packets    Bytes        Tx Packets   Rx Packets   Tx Bytes     Rx Bytes     Tx Util    Rx Util
-------------------------------------------------------------------------------------------------------------------
192.168.1.100     2500       500000       1500         1000         300000       200000       0.5%       0.3%
8.8.8.8           1800       350000       900          900          175000       175000       0.3%       0.3%
192.168.1.1       1200       200000       600          600          100000       100000       0.2%       0.2%
...
```


## Reflection Questions

1.  How can identifying "top talkers" help in troubleshooting network performance issues?
2.  What might a large discrepancy between "Tx Packets" and "Rx Packets" for a particular endpoint indicate?
3.  How could this feature be used in a security context to identify suspicious activity, such as data exfiltration or botnet activity?

## Next Steps

*   [Exercise 13: Rogue DHCP Server](13-rogue-dhcp-server.md)
*   Further reading: [Wireshark Wiki: Endpoints](https://wiki.wireshark.org/Endpoints)

## Hints / Troubleshooting

*   If your "Endpoints" list seems empty or shows unexpected IPs, ensure your capture was active for long enough and on the correct interface.
*   The "Endpoints" window can also show "Ethernet" and "UDP" tabs, which might be useful for different types of analysis.
*   Remember to refresh the statistics if your capture is still running (`Analyze > Reload` or `Ctrl+R`).
