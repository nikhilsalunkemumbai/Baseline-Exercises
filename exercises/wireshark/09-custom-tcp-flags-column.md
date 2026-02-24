---
Title: Exercise 09: Custom TCP Flags Column
Objective: Customize the Wireshark display to add a custom column showing the TCP flags for each packet, enabling quick visual identification of SYN, ACK, FIN, RST, and other flag combinations.
Estimated Time: 15-25 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file with TCP traffic).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Generate some TCP traffic (e.g., visit a few HTTPS websites, SSH into a server, or transfer a small file). Stop the capture.
4. **Option B (Existing PCAP):** Use a pre-captured PCAP file that contains various TCP traffic.
---

## Steps

1.  **Open the PCAP File in Wireshark (or use Live Capture):**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file, or start and stop a live capture as described in Setup.

2.  **Access Wireshark Preferences:**
    *   Go to `Edit > Preferences` (on Windows/Linux) or `Wireshark > Preferences` (on macOS).

3.  **Navigate to Columns Settings:**
    *   In the Preferences window, expand "Appearance" and select "Columns".

4.  **Add a New Custom Column:**
    *   Click the `+` button at the bottom left of the "Columns" list to add a new column.
    *   **Title:** Enter a descriptive name for the new column, e.g., `TCP Flags`.
    *   **Type:** Change the "Type" dropdown to `Custom`.
    *   **Field:** In the "Field" input box, type `tcp.flags`.
    *   (Optional) You can change the "Align" setting to `Center` for better readability.

5.  **Reorder Column (Optional):**
    *   You can drag and drop the newly created "TCP Flags" column to a desired position in the list (e.g., next to the "Info" column).

6.  **Apply Changes and Observe:**
    *   Click `OK` to close the Preferences window.
    *   Observe the packet list pane. You should now see the new "TCP Flags" column, displaying the hexadecimal value and the set flags for each TCP packet (e.g., `0x002 (SYN)`, `0x018 (PSH, ACK)`).

7.  **Filter and Verify (Optional):**
    *   Apply a filter like `tcp` to see only TCP traffic.
    *   Scroll through the packets and quickly identify common flags like SYN (for connection initiation), SYN/ACK (for server response), ACK (for acknowledgment), FIN (for connection termination), and RST (for connection reset).

## Expected Output

```
(A new column titled "TCP Flags" will appear in the packet list pane, showing the decoded TCP flags for each relevant packet.)

Example of Packet List with "TCP Flags" column:
No.     Time        Source          Destination     Protocol Length Info                     TCP Flags
1       0.000000    192.168.1.100   1.2.3.4         TCP      74     50000 > 80 [SYN] Seq=0...     0x002 (SYN)
2       0.000050    1.2.3.4         192.168.1.100   TCP      74     80 > 50000 [SYN, ACK] Seq=0...  0x012 (SYN, ACK)
3       0.000100    192.168.1.100   1.2.3.4         TCP      66     50000 > 80 [ACK] Seq=1...     0x010 (ACK)
4       0.000150    192.168.1.100   1.2.3.4         HTTP     250    GET /index.html HTTP/1.1      0x018 (PSH, ACK)
...
```
![Expected output](images/exercise-09-output.png)

## Reflection Questions

1.  How does having a dedicated "TCP Flags" column improve efficiency in analyzing TCP handshakes compared to relying solely on the "Info" column?
2.  Which specific TCP flag (or combination of flags) would you look for to quickly identify:
    *   The start of a new connection?
    *   An abrupt termination of a connection?
3.  Can this custom column help in detecting network anomalies like SYN floods or retransmissions? If so, how?

## Next Steps

*   [Exercise 10: Detect ARP Spoofing](10-detect-arp-spoofing.md)
*   Further reading: [Wireshark Display Filter Reference: TCP Flags](https://www.wireshark.org/docs/dfref/t/tcp.flags.html)

## Hints / Troubleshooting

*   If the new column appears but shows no data, ensure you have TCP traffic in your capture. Filter for `tcp` to verify.
*   Make sure you correctly set the "Type" to `Custom` and the "Field" to `tcp.flags` in the column preferences.
*   The hexadecimal values for flags are bitmasks. For example, `0x002` means only the SYN flag is set. `0x012` means SYN (0x002) and ACK (0x010) are set.
