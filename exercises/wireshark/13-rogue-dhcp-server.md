---
Title: Exercise 13: Rogue DHCP Server
Objective: Detect the presence of a rogue DHCP server on a network by identifying multiple DHCP Offer packets originating from different (unauthorized) MAC addresses for the same DHCP request.
Estimated Time: 30-45 minutes
Tools Needed: Wireshark, a network environment where a rogue DHCP server can be simulated (or a PCAP containing such a simulation).
Setup Instructions: 
1. Install Wireshark on your system.
2. **Simulate a Rogue DHCP Server (in a controlled Lab Environment only):**
    *   Set up a lab network with at least one legitimate DHCP server and one client device.
    *   Introduce a second, *unauthorized* (rogue) DHCP server into the network. This could be:
        *   A misconfigured router acting as a DHCP server.
        *   A Kali Linux VM with a DHCP server (`isc-dhcp-server`) configured to offer IPs.
        *   A Windows server with the DHCP role installed but not properly integrated or authorized.
    *   Start a Wireshark capture on a network interface that can observe traffic from both DHCP servers and the client.
    *   On the client device, force it to request a new IP address:
        *   Windows: Open Command Prompt and type `ipconfig /release` then `ipconfig /renew`.
        *   Linux: Open Terminal and type `sudo dhclient -r` then `sudo dhclient`.
    *   Stop the Wireshark capture after the client has received an IP address.
3. **Use a Sample PCAP:** Search for publicly available PCAP samples that demonstrate rogue DHCP server activity. *Always exercise caution and use only in isolated lab environments.*
---

## Steps

1.  **Open the PCAP File in Wireshark:**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file.

2.  **Filter for DHCP Traffic:**
    *   In the display filter bar, type `dhcp` (or `bootp`) and press Enter. This will display all DHCP-related packets (Discover, Offer, Request, ACK).

3.  **Identify DHCP Discover and Offer Packets:**
    *   Scroll through the filtered packets.
    *   Look for a `DHCP Discover` packet (client looking for DHCP servers). Note the "Transaction ID" (xid) in the "Info" column, or inspect it in the packet details pane.
    *   Immediately after the `DHCP Discover`, look for `DHCP Offer` packets. These are responses from DHCP servers offering IP configurations.

4.  **Look for Multiple DHCP Offers for a Single Discover:**
    *   For a single `DHCP Discover` (identified by its unique "Transaction ID"), observe if there are `DHCP Offer` packets coming from *different Source MAC addresses* or *different Source IP addresses*.
    *   A legitimate network should typically have only one DHCP server responding with an Offer for a given Discover. Multiple Offers from different servers are a strong indicator of a rogue DHCP server.

5.  **Examine MAC Addresses of Offering Servers:**
    *   For each `DHCP Offer` packet, expand the "Ethernet II" layer in the packet details pane to see the "Source MAC address".
    *   Also, expand the "Internet Protocol Version 4" layer to see the "Source IP address".
    *   Compare these source addresses with known legitimate DHCP servers on your network. Offers from unknown MAC/IP addresses are suspicious.

6.  **Use Wireshark's "Expert Information" (Optional):**
    *   Go to `Analyze > Expert Information`. Wireshark might flag potential issues like "DHCP: Multiple DHCPOFFERs" if detected.

## Expected Output

```
(Expected output will show a DHCP Discover packet from a client, followed by multiple DHCP Offer packets from different server MAC/IP addresses for the same Transaction ID.)

Example in Wireshark Packet List (after applying `dhcp` filter):
No.     Time        Source          Destination     Protocol Length Info                     
10      0.000100    00:11:22:33:44:55 Broadcast       DHCP     342    DHCP Discover (xid=0x12345678)
15      0.000500    00:55:66:77:88:99 00:11:22:33:44:55 DHCP     302    DHCP Offer (xid=0x12345678)
18      0.000600    AA:BB:CC:DD:EE:FF 00:11:22:33:44:55 DHCP     302    DHCP Offer (xid=0x12345678) <-- Rogue!

In Packet Details Pane (for the rogue DHCP Offer packet, No. 18):
Frame 18: ...
Ethernet II, Src: AA:BB:CC:DD:EE:FF, Dst: 00:11:22:33:44:55
Internet Protocol Version 4, Src: 192.168.1.5, Dst: 192.168.1.100
User Datagram Protocol, Src Port: 67, Dst Port: 68
Bootstrap Protocol (DHCP)
    Message type: Boot Reply (2)
    Hardware type: Ethernet (1)
    Hardware address length: 6
    Hops: 0
    Transaction ID: 0x12345678
    ...
    Option: (53) DHCP Message Type (DHCP Offer)
    Option: (54) DHCP Server Identifier (192.168.1.5)
    ...
```


## Reflection Questions

1.  What are the four main steps in the DHCP DORA process (Discover, Offer, Request, ACK), and what role does each play?
2.  How could a rogue DHCP server be used in a Man-in-the-Middle (MITM) attack?
3.  What are immediate actions an administrator should take upon detecting a rogue DHCP server?

## Next Steps

*   [Exercise 14: HTTP User-Agents](14-http-user-agents.md)
*   Further reading: [Wireshark Wiki: DHCP](https://wiki.wireshark.org/DHCP)

## Hints / Troubleshooting

*   If you don't see multiple offers, your rogue DHCP server might not be configured correctly, or your client isn't sending a new Discover.
*   Ensure that the "Source" MAC address in the `DHCP Offer` packets actually corresponds to the server (or device) sending the offer, not just the IP it's offering.
*   The transaction ID (xid) is crucial for matching Discover and Offer packets. Make sure offers have the same xid as the discover.
