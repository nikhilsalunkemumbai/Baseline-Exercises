---
Title: Exercise 06: Follow TCP Stream
Objective: Learn to use Wireshark's "Follow TCP Stream" feature to reconstruct and view the complete conversation of an HTTP session.
Estimated Time: 20-30 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file with unencrypted HTTP traffic).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Browse to a non-HTTPS website (e.g., `http://example.com` or a local web server), interact with it briefly (e.g., click a link), and then stop the capture.
4. **Option B (Existing PCAP):** Use a pre-captured PCAP file that contains unencrypted HTTP traffic. (e.g., from Exercise 01: Capture and Inspect an HTTP Request).
---

## Steps

1.  **Open the PCAP File in Wireshark (or use Live Capture):**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file, or start and stop a live capture as described in Setup.

2.  **Filter for HTTP Traffic:**
    *   In the display filter bar, type `http` and press Enter. This will display all HTTP requests and responses.

3.  **Select an HTTP Packet to Follow:**
    *   Scroll through the filtered packets and find an `HTTP GET` request (or any HTTP packet that is part of a conversation you want to examine).
    *   Right-click on the selected `HTTP GET` packet.

4.  **Follow the TCP Stream:**
    *   From the context menu, select `Follow > TCP Stream`.
    *   A new window will open, displaying the reconstructed TCP stream for that specific conversation. This window typically shows the client's requests in red and the server's responses in blue, making it easy to read the full dialogue.

5.  **Inspect the Conversation:**
    *   Read through the conversation in the "Follow TCP Stream" window. You should see the complete HTTP request (headers and body if present) and the server's HTTP response (status line, headers, and the HTML/content body).
    *   You can change the "Show data as" option (e.g., ASCII, EBCDIC, Hex Dump, Raw) to view the data in different formats. For HTTP, ASCII or UTF-8 is usually most readable.

6.  **Close the TCP Stream Window:**
    *   Click the "Close" button to close the "Follow TCP Stream" window and return to the main Wireshark interface.

## Expected Output

```
(The "Follow TCP Stream" window will pop up, showing the complete HTTP request and response exchanged between the client and server. The content of the webpage might be visible in plain text.)

Example of "Follow TCP Stream" content:
============================================================
GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Upgrade-Insecure-Requests: 1

HTTP/1.1 200 OK
Date: Tue, 24 Feb 2026 10:00:00 GMT
Server: ECS (dca/2443)
Last-Modified: Sun, 16 May 2024 16:30:00 GMT
ETag: "1b03-611d2e1f5d600"
Accept-Ranges: bytes
Content-Length: 6915
Content-Type: text/html

<!doctype html>
<html>
<head>
    <title>Example Domain</title>
... (HTML content of the page)
</html>
============================================================
```
![Expected output](images/exercise-06-output.png)

## Reflection Questions

1.  What information in the HTTP request headers could be used to identify the client's browser or operating system?
2.  How would you identify potential data exfiltration in an HTTP stream if a user was uploading a file?
3.  What are the limitations of "Follow TCP Stream" when dealing with encrypted traffic like HTTPS?

## Next Steps

*   [Exercise 07: TLS Handshake Inspection](07-tls-handshake-inspection.md)
*   Further reading: [Wireshark Wiki: TCP Stream Graph](https://wiki.wireshark.org/TCP_Stream_Graphs)

## Hints / Troubleshooting

*   If "Follow TCP Stream" shows encrypted or unreadable data, you likely selected an HTTPS stream. Ensure you are analyzing unencrypted HTTP traffic for this exercise.
*   If you can't find clear HTTP traffic, try browsing a known non-HTTPS site like `http://testphp.vulnweb.com` (use with caution and only for educational purposes in a lab).
*   Sometimes, multiple TCP streams might be associated with a single web page load (e.g., for images, scripts). Pick a primary `GET` request for the main page content.
