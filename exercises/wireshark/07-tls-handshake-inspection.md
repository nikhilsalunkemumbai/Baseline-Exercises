---
Title: Exercise 07: TLS Handshake Inspection
Objective: Inspect the TLS Client Hello message in a Wireshark capture to identify the Server Name Indication (SNI) and proposed cipher suites, crucial for understanding secure connection establishment.
Estimated Time: 30-45 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file with HTTPS traffic).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Browse to several different HTTPS websites (e.g., `https://www.google.com`, `https://www.wikipedia.org`, `https://www.github.com`) to generate TLS handshake traffic. Stop the capture after browsing for a few minutes.
4. **Option B (Existing PCAP):** Use a pre-captured PCAP file that contains HTTPS (TLS) traffic.
---

## Steps

1.  **Open the PCAP File in Wireshark (or use Live Capture):**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file, or start and stop a live capture as described in Setup.

2.  **Filter for TLS Client Hello Messages:**
    *   In the display filter bar, type `tls.handshake.type == 1` and press Enter. This filter will show only TLS Client Hello messages, which are the first messages sent by the client to initiate a TLS handshake.

3.  **Inspect a Client Hello Message:**
    *   Select one of the `Client Hello` packets in the packet list pane.
    *   In the packet details pane, expand the "Transport Layer Security" layer.
    *   Further expand "TLSv1.x Record Layer: Handshake Protocol: Client Hello" (where `x` is the TLS version).
    *   Look for the "Handshake Protocol: Client Hello" section.

4.  **Identify Server Name Indication (SNI):**
    *   Within the "Handshake Protocol: Client Hello" section, scroll down and expand "Extension: server_name (host_name)".
    *   The `Server Name` field will show the hostname the client is trying to connect to (e.g., `www.google.com`). This is the SNI.

5.  **Examine Proposed Cipher Suites:**
    *   Within the "Handshake Protocol: Client Hello" section, locate and expand "Cipher Suites".
    *   This list will show the encryption algorithms and key exchange mechanisms the client supports and proposes to the server. Note some of the names (e.g., `TLS_AES_256_GCM_SHA384`, `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`).

## Expected Output

```
(The Wireshark packet list will show TLS Client Hello messages. When a Client Hello packet is selected, the packet details pane will show the SNI hostname and a list of supported cipher suites.)

Example in Wireshark Packet List (after applying `tls.handshake.type == 1`):
No.     Time        Source          Destination     Protocol Length Info
105     10.512345   192.168.1.100   142.250.72.106  TLSv1.3  198    Client Hello

In Packet Details Pane (for selected Client Hello):
Frame 105: ...
Ethernet II, Src: ...
Internet Protocol Version 4, Src: 192.168.1.100, Dst: 142.250.72.106
Transmission Control Protocol, Src Port: 55482, Dst Port: 443, Seq: 1, Ack: 1, Len: 130
Transport Layer Security
    TLSv1.3 Record Layer: Handshake Protocol: Client Hello
        Handshake Protocol: Client Hello
            Version: TLS 1.2 (0x0303) (Note: Client Hello often lists max supported TLS version, but might negotiate down)
            Random: ...
            Session ID Length: 0
            Cipher Suites Length: 44 (22 suites)
            Cipher Suites (22 suites)
                Cipher Suite: TLS_AES_256_GCM_SHA384 (0x1302)
                Cipher Suite: TLS_CHACHA20_POLY1305_SHA256 (0x1303)
                Cipher Suite: TLS_AES_128_GCM_SHA256 (0x1301)
                ...
            Compression Methods Length: 1
            Compression Methods: null (0x00)
            Extensions Length: 100
            Extension: server_name (host_name)
                Type: server_name (0x0000)
                Length: 19
                Server Name Indication extension
                    Server Name: example.com (Type: host_name (0))
            Extension: supported_versions
                Type: supported_versions (0x002b)
                Length: 5
                Supported Versions: TLS 1.3 (0x0304), TLS 1.2 (0x0303), TLS 1.1 (0x0302)
            ...
```
![Expected output](images/exercise-07-output.png)

## Reflection Questions

1.  Why is the Server Name Indication (SNI) extension important in TLS handshakes, especially when multiple websites are hosted on the same IP address?
2.  How might an attacker leverage information about supported cipher suites during a reconnaissance phase?
3.  What does it mean if you see a `Client Hello` with an older TLS version (e.g., TLS 1.0 or TLS 1.1) being proposed by default?

## Next Steps

*   [Exercise 08: MAC Address Filter](08-mac-address-filter.md)
*   Further reading: [Wireshark Display Filter Reference: TLS](https://www.wireshark.org/docs/dfref/t/tls.html)

## Hints / Troubleshooting

*   If you don't see any TLS traffic, ensure you visited HTTPS websites.
*   The `tls.handshake.type == 1` filter specifically targets the Client Hello message. If you want to see all TLS traffic, simply use `tls`.
*   The actual TLS version number (e.g., TLSv1.2, TLSv1.3) displayed in the "Info" column or packet details might vary based on your client and the server.
