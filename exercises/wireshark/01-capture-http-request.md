---
Title: Exercise 01: Capture and Inspect an HTTP Request
Objective: Learn to start a Wireshark capture, browse to a website, stop the capture, and filter HTTP traffic to inspect the request and response.
Estimated Time: 30-45 minutes
Tools Needed: Wireshark, Web Browser
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
---

## Steps

1.  **Open Wireshark and Start Capture:**
    *   Launch Wireshark.
    *   Select your active network interface (e.g., Wi-Fi or Ethernet adapter) from the main screen.
    *   Click the "Start capturing packets" button (usually a shark fin icon or "Start" button) to begin a new capture.

2.  **Browse to a Website:**
    *   Open your web browser.
    *   Navigate to a non-HTTPS website (e.g., `http://example.com` or a local web server if you have one set up for testing). Visiting an unencrypted site will make HTTP traffic visible.

3.  **Stop Capture:**
    *   Return to Wireshark.
    *   Click the "Stop capturing packets" button (usually a red square icon).

4.  **Apply HTTP Display Filter:**
    *   In the Wireshark display filter bar (at the top, above the packet list pane), type `http` and press Enter.

5.  **Inspect HTTP Traffic:**
    *   Look for `HTTP GET` requests and `HTTP/1.1 200 OK` responses.
    *   Click on an `HTTP GET` request, then expand the "Hypertext Transfer Protocol" section in the packet details pane to view request headers (e.g., Host, User-Agent).
    *   Click on the corresponding `HTTP/1.1 200 OK` response, then expand the "Hypertext Transfer Protocol" section to view response headers (e.g., Server, Content-Type).

## Expected Output

```
(Expected output will show a list of filtered HTTP packets. You should see GET requests and 200 OK responses. The packet details pane will show dissected HTTP headers.)

Example of HTTP GET Request in packet list:
No.     Time        Source          Destination     Protocol Length Info
23      1.234567    192.168.1.100   93.184.216.34   HTTP     250    GET / HTTP/1.1

Example of HTTP/1.1 200 OK Response in packet list:
No.     Time        Source          Destination     Protocol Length Info
24      1.245678    93.184.216.34   192.168.1.100   HTTP     800    HTTP/1.1 200 OK  (text/html)

In Packet Details Pane (for GET request):
Hypertext Transfer Protocol
    GET / HTTP/1.1

    Host: example.com

    User-Agent: Mozilla/5.0 (...)

    Accept: text/html (...)

    ...

In Packet Details Pane (for 200 OK response):
Hypertext Transfer Protocol
    HTTP/1.1 200 OK

    Date: ...

    Server: ECS (dca/2443)

    Content-Length: 1256

    Content-Type: text/html

    ...
```
![Expected output](images/exercise-01-output.png)

## Reflection Questions

1.  What information can you identify from the `Host` header in an HTTP request?
2.  How would the capture differ if you visited an HTTPS website instead of an HTTP website?
3.  Why is filtering traffic important in Wireshark for analysis?

## Next Steps

*   [Exercise 02: Filter by IP](02-filter-by-ip.md)
*   Further reading: [Wireshark Display Filter Reference](https://www.wireshark.org/docs/dfref/)

## Hints / Troubleshooting

*   If you don't see any HTTP traffic, ensure you visited a non-HTTPS website. Many modern browsers force HTTPS connections by default.
*   Verify you selected the correct network interface for capture.
*   If your display filter doesn't work, check for typos (e.g., `http` vs. `HTTP`). Wireshark display filters are generally case-insensitive for protocol names but sensitive for some field values.
