---
Title: Exercise 10: Detect ARP Spoofing
Objective: Detect Address Resolution Protocol (ARP) spoofing by identifying multiple different MAC addresses claiming the same IP address within ARP packets in a Wireshark capture.
Estimated Time: 45-60 minutes
Tools Needed: Wireshark, PCAP file with (simulated) ARP spoofing traffic.
Setup Instructions: 
1. Install Wireshark on your system.
2. **Obtain a PCAP file with ARP spoofing (Highly Recommended in a Lab Environment):**
    *   **Simulate your own:** Set up a controlled lab environment with at least three machines:
        *   **Attacker Machine:** (e.g., Kali Linux with `arpspoof`)
        *   **Victim Machine:** (any OS, e.g., Windows VM)
        *   **Gateway/Router:** (e.g., a virtual router or another VM acting as a gateway)
        *   **Monitoring Machine:** (the machine where you run Wireshark, ideally connected to a switch where traffic from the attacker/victim can be observed, or using port mirroring).
        *   **Steps to simulate (example using `arpspoof` on Kali):**
            1.  Start Wireshark capture on your monitoring machine.
            2.  On the attacker machine, enable IP forwarding: `echo 1 > /proc/sys/net/ipv4/ip_forward`
            3.  On the attacker machine, run `arpspoof -i <attacker_interface> -t <victim_IP> <gateway_IP>` (e.g., `arpspoof -i eth0 -t 192.168.1.10 192.168.1.1`).
            4.  Simultaneously, run `arpspoof -i <attacker_interface> -t <gateway_IP> <victim_IP>` (e.g., `arpspoof -i eth0 -t 192.168.1.1 192.168.1.10`). This redirects traffic between victim and gateway through the attacker.
            5.  Generate some traffic from the victim (e.g., browse a website).
            6.  Stop the Wireshark capture.
    *   **Use a sample:** Search for publicly available PCAP samples that include ARP spoofing. *Always exercise caution and use only in isolated lab environments.*

---

## Steps

1.  **Open the PCAP File in Wireshark:**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file containing the ARP spoofing traffic.

2.  **Filter for ARP Traffic:**
    *   In the display filter bar, type `arp` and press Enter. This will show all ARP-related packets.

3.  **Analyze ARP Requests/Responses for Duplicates:**
    *   Scroll through the packets in the packet list pane.
    *   Look for `ARP Reply` or `ARP Gratuitous` packets.
    *   Pay close attention to the "Sender MAC address" and "Sender IP address" fields in the "Info" column, or inspect them in the packet details pane under "Address Resolution Protocol".
    *   **The key indicator:** You are looking for instances where the *same IP address* is associated with *different MAC addresses* in a short period. For example, if `192.168.1.1` (your gateway) suddenly appears to be at a MAC address other than its legitimate one.

4.  **Use Wireshark's Built-in Feature for ARP Duplicates:**
    *   Wireshark has a specific filter to detect this: `arp.duplicate-address-frame == 1`. This filter highlights ARP packets that claim an IP address that another device has already claimed.
    *   Apply this filter and observe the results. Packets matching this criteria will be displayed.

5.  **Examine Packet Details:**
    *   Select a packet identified by the `arp.duplicate-address-frame` filter.
    *   In the packet details pane, expand "Address Resolution Protocol".
    *   Note the "Sender MAC address" and "Sender IP address" values. You might see different MAC addresses sending ARP replies for the same IP.

6.  **Use `Statistics > Endpoints` (Optional):**
    *   Go to `Statistics > Endpoints`.
    *   Select the "Ethernet" tab.
    *   Look for IP addresses that are associated with multiple different MAC addresses. This can visually confirm the presence of duplicate claims.

## Expected Output

```
(Expected output will show ARP packets. When using the `arp.duplicate-address-frame == 1` filter, you should see packets indicating that a MAC address is attempting to claim an IP address already in use by another MAC.)

Example in Wireshark Packet List (after applying `arp.duplicate-address-frame == 1`):
No.     Time        Source          Destination     Protocol Length Info                     
120     10.512345   00:11:22:33:44:55 Broadcast       ARP      42     Gratuitous ARP for 192.168.1.1 (Request)
121     10.512400   AA:BB:CC:DD:EE:FF Broadcast       ARP      42     Gratuitous ARP for 192.168.1.1 (Request) [Duplicate IP address]

In Packet Details Pane (for an identified duplicate packet):
Frame 121: ...
Ethernet II, Src: AA:BB:CC:DD:EE:FF, Dst: Broadcast
Address Resolution Protocol (request)
    Hardware type: Ethernet (1)
    Protocol type: IPv4 (0x0800)
    Hardware size: 6
    Protocol size: 4
    Opcode: request (1)
    Sender MAC address: AA:BB:CC:DD:EE:FF
    Sender IP address: 192.168.1.1
    Target MAC address: 00:00:00:00:00:00 (broadcast)
    Target IP address: 192.168.1.1
```


## Reflection Questions

1.  How does ARP spoofing work at a fundamental level, and what is its primary goal for an attacker?
2.  What immediate impact would ARP spoofing have on network communication, and why might it be difficult for a user to detect without tools like Wireshark?
3.  What are some common countermeasures against ARP spoofing, and how do they prevent such attacks?

## Next Steps

*   [Exercise 11: IO Graph](11-io-graph.md)
*   Further reading: [Wireshark Wiki: ARP Spoofing Detection](https://wiki.wireshark.org/ARP_Spoofing_Detection)

## Hints / Troubleshooting

*   If you're simulating ARP spoofing and don't see duplicate frames, ensure IP forwarding is enabled on your attacker machine and that `arpspoof` (or similar tool) is actively running correctly.
*   Verify that your Wireshark capture is on an interface that can see the broadcast ARP traffic.
*   Sometimes, network switches with ARP inspection features can mitigate spoofing, making it harder to capture in a PCAP. Ensure your lab setup allows the spoofing to occur.
*   Wireshark uses different colors to highlight abnormal packets. Look for expert info messages at the bottom of the window.
