---
Title: Exercise 08: MAC Address Filter
Objective: Filter Wireshark traffic to display all packets to and from a specific MAC address, allowing you to isolate and analyze communication from a particular network device.
Estimated Time: 15-25 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Identify a MAC Address:** Find the MAC address of a device on your local network.
    *   **Your own machine:**
        *   Windows: Open Command Prompt and type `ipconfig /all`. Look for "Physical Address" under your active network adapter.
        *   Linux/macOS: Open Terminal and type `ifconfig` or `ip a`. Look for the MAC address next to `ether` or `link/ether`.
    *   **Another device/router:** You might find this in your router's administration interface or by inspecting traffic in Wireshark (before filtering) and noting a source/destination MAC.
4. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Generate some traffic from/to the device with the identified MAC address. Stop the capture.
5. **Option B (Existing PCAP):** Use a pre-captured PCAP file that contains traffic from the MAC address you wish to filter.
---

## Steps

1.  **Open the PCAP File in Wireshark (or use Live Capture):**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file, or start and stop a live capture as described in Setup.

2.  **Apply the MAC Address Display Filter:**
    *   In the display filter bar, type `eth.addr == AA:BB:CC:DD:EE:FF` (replace `AA:BB:CC:DD:EE:FF` with the target MAC address you identified).
    *   Press Enter to apply the filter.

3.  **Inspect Filtered Traffic:**
    *   Observe the packet list pane. All displayed packets should now show your specified MAC address as either the "Source" or "Destination" in the "Source" and "Destination" columns.
    *   Examine the "Protocol" and "Info" columns to understand the types of traffic associated with this MAC address.

4.  **Refine Filter (Optional):**
    *   To see traffic *from* a specific MAC address: `eth.src == AA:BB:CC:DD:EE:FF`
    *   To see traffic *to* a specific MAC address: `eth.dst == AA:BB:CC:DD:EE:FF`

## Expected Output

```
(Expected output will show only packets where the specified MAC address is either the source or destination Ethernet address. The example below uses a placeholder MAC.)

Example of Filtered Packet List:
No.     Time        Source          Destination     Protocol Length Info
5       0.000123    AA:BB:CC:DD:EE:FF 192.168.1.1     ARP      42     Who has 192.168.1.1? Tell 192.168.1.100
6       0.000345    192.168.1.1     AA:BB:CC:DD:EE:FF ARP      42     192.168.1.1 is at 00:11:22:33:44:55
12      0.123456    AA:BB:CC:DD:EE:FF 8.8.8.8         DNS      75     Standard query A www.google.com
13      0.123890    8.8.8.8         AA:BB:CC:DD:EE:FF DNS      75     Standard query response A 142.250.72.36
... (all packets will involve AA:BB:CC:DD:EE:FF as either source or destination)
```


## Reflection Questions

1.  What layer of the OSI model does MAC address filtering operate on, and why is this important?
2.  How can MAC address filtering be useful in detecting ARP spoofing attempts?
3.  What are the limitations of MAC address filtering in a routed network compared to a local segment?

## Next Steps

*   [Exercise 09: Custom TCP Flags Column](09-custom-tcp-flags-column.md)
*   Further reading: [Wireshark Display Filter Reference: Ethernet](https://www.wireshark.org/docs/dfref/e/eth.html)

## Hints / Troubleshooting

*   Ensure the MAC address is entered in the correct format (e.g., `AA:BB:CC:DD:EE:FF` or `AA-BB-CC-DD-EE-FF`). Wireshark is usually flexible with separators.
*   If no packets appear, double-check the MAC address for accuracy. MAC addresses are often specific to network adapters.
*   If you're in a switched network, ensure your capture method (e.g., direct connection, SPAN port, ARP poisoning) allows you to see traffic not directly addressed to your NIC.
