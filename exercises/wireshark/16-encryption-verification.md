---
Title: Exercise 16: Encryption Verification
Objective: Verify that a Hypertext Transfer Protocol Secure (HTTPS) connection is correctly using Transport Layer Security (TLS) by confirming the presence of TLS handshake messages and the absence of plaintext HTTP communication.
Estimated Time: 20-30 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file with HTTPS traffic).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Browse to a secure (HTTPS) website (e.g., `https://www.google.com`, `https://www.github.com`, your banking portal). Interact briefly with the site to generate traffic. Stop the capture.
4. **Option B (Existing PCAP):** Use a pre-captured PCAP file that contains HTTPS (TLS) traffic.
---

## Steps

1.  **Open the PCAP File in Wireshark:**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file.

2.  **Filter for TLS Handshake Traffic:**
    *   In the display filter bar, type `tls.handshake` and press Enter. This will show all packets involved in the TLS handshake process.
    *   Look for `Client Hello`, `Server Hello`, `Certificate`, `Server Key Exchange`, and `Client Key Exchange` messages. Their presence indicates that a TLS handshake occurred.

3.  **Verify Absence of Plaintext HTTP:**
    *   Remove the `tls.handshake` filter and apply the filter `http`.
    *   If the HTTPS connection is working correctly, you should see little to no HTTP traffic (packets with Protocol `HTTP`) related to your secure browsing session. If you see HTTP traffic, it indicates potential downgrade attacks or that you are indeed visiting an HTTP site.

4.  **Inspect Certificate Details (Optional - requires decryption or specific PCAP):**
    *   If you have a PCAP file where TLS decryption is possible (e.g., by providing the server's private key or `SSLKEYLOGFILE` for browser traffic), you could inspect the "TLSv1.x Certificate" message.
    *   In a `Certificate` message, expand "Transport Layer Security" -> "Handshake Protocol: Certificate" -> "Certificates" -> "X.509Sertificate". You can then see details like "Subject", "Issuer", "Valid From", and "Valid To". For this exercise, confirming the TLS handshake and absence of HTTP is sufficient.

5.  **Look for TLS Alerts (Optional):**
    *   Filter for `tls.alert_message`. TLS alert messages can indicate issues with the secure connection, such as certificate warnings or decryption failures.

## Expected Output

```
(Expected output will demonstrate TLS handshake messages, and when filtered for HTTP, minimal or no HTTP traffic related to the HTTPS session. For certificate details, you'd need a decrypted PCAP.)

Example 1: Packet List with `tls.handshake` filter showing TLS handshake sequence.
No.     Time        Source          Destination     Protocol Length Info
10      0.000100    192.168.1.100   1.2.3.4         TLSv1.2  198    Client Hello
11      0.000150    1.2.3.4         192.168.1.100   TLSv1.2  200    Server Hello
12      0.000200    1.2.3.4         192.168.1.100   TLSv1.2  1200   Certificate
13      0.000250    1.2.3.4         192.168.1.100   TLSv1.2  120    Server Key Exchange
14      0.000300    192.168.1.100   1.2.3.4         TLSv1.2  200    Client Key Exchange
15      0.000350    192.168.1.100   1.2.3.4         TLSv1.2  80     Change Cipher Spec, Encrypted Handshake Message
16      0.000400    1.2.3.4         192.168.1.100   TLSv1.2  80     Change Cipher Spec, Encrypted Handshake Message

Example 2: Packet List with `http` filter (related to the same session), showing no packets.
(No packets displayed if HTTPS is correctly enforced)
```


## Reflection Questions

1.  What are the key messages exchanged during a TLS handshake, and what is the purpose of each?
2.  How would the absence of `Client Hello` or `Server Hello` messages indicate an issue with encryption, even if HTTP traffic isn't seen?
3.  What is the significance of the padlock icon in your browser's address bar, and how does it relate to what you've observed in Wireshark?

## Next Steps

*   Proceed to Sysinternals Exercises.
*   Further reading: [Wireshark Wiki: TLS](https://wiki.wireshark.org/TLS)

## Hints / Troubleshooting

*   If you see `HTTP` traffic when browsing an `HTTPS` site, it might indicate a redirection chain or a non-secure element being loaded on an otherwise secure page. This often appears as `HTTP/1.1 301 Moved Permanently` or `302 Found` followed by an `HTTPS` redirect.
*   TLS traffic often appears simply as `TLSv1.x` in the Protocol column, with "Application Data" in the Info column for encrypted data. You won't see the actual application-layer data without decryption.
*   For advanced TLS analysis (e.g., certificate validation, decryption), refer to Wireshark's documentation on TLS decryption (usually requires `SSLKEYLOGFILE` environment variable for browser traffic or server private keys for server traffic in lab environments).
