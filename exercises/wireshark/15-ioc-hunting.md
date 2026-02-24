---
Title: Exercise 15: IOC Hunting
Objective: Filter Wireshark traffic to identify any communication (as source or destination) involving a predefined list of malicious IP addresses (Indicators of Compromise - IOCs).
Estimated Time: 20-30 minutes
Tools Needed: Wireshark, PCAP file, a text file (`malicious_ips.txt`) containing a list of IP addresses.
Setup Instructions: 
1. Install Wireshark on your system.
2. **Create a `malicious_ips.txt` file:**
    *   Create a simple text file named `malicious_ips.txt` (e.g., on your desktop).
    *   Populate it with a few IP addresses, one per line. These can be real or dummy IPs for testing purposes. Example:
        ```
        1.1.1.1
        10.0.0.5
        192.168.1.200
        ```
3. **Obtain a PCAP file:**
    *   For testing, any PCAP file with network traffic will work.
    *   For a more realistic scenario, you might use a PCAP from a network segment where suspicious activity is suspected.
---

## Steps

1.  **Prepare the Malicious IP List:**
    *   Ensure your `malicious_ips.txt` file is accessible.

2.  **Open the PCAP File in Wireshark:**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file.

3.  **Construct the Display Filter from the IP List:**
    *   Wireshark does not directly import IP lists for filtering. You'll need to manually (or programmatically for very long lists) construct a filter using `or` operators.
    *   For each IP address in `malicious_ips.txt`, add an `ip.addr == X.X.X.X` clause, joined by `or`.
    *   **Example filter string for `malicious_ips.txt` above:**
        `ip.addr == 1.1.1.1 or ip.addr == 10.0.0.5 or ip.addr == 192.168.1.200`

4.  **Apply the Filter in Wireshark:**
    *   Copy the constructed filter string into the Wireshark display filter bar.
    *   Press Enter to apply the filter.

5.  **Inspect Filtered Traffic:**
    *   Observe the packet list pane. Only packets where the source or destination IP address matches one of your specified malicious IPs should be displayed.
    *   Examine the "Protocol" and "Info" columns for any communication involving these IOCs.

6.  **Refine Filter for Source/Destination (Optional):**
    *   To specifically look for outbound connections *to* these IPs:
        `ip.dst == 1.1.1.1 or ip.dst == 10.0.0.5 or ip.dst == 192.168.1.200`
    *   To specifically look for inbound connections *from* these IPs:
        `ip.src == 1.1.1.1 or ip.src == 10.0.0.5 or ip.src == 192.168.1.200`

## Expected Output

```
(The Wireshark packet list will display only packets that originate from or are destined for any of the IP addresses specified in your malicious IP list. You might see DNS queries, HTTP requests, or other protocol traffic if there was communication.)

Example of Filtered Packet List (with dummy malicious IPs):
No.     Time        Source          Destination     Protocol Length Info
10      0.000100    192.168.1.100   1.1.1.1         DNS      75     Standard query A badsite.com
11      0.000150    10.0.0.5        192.168.1.100   ICMP     98     Echo (ping) request  id=0x0001, seq=1/256, ttl=128
12      0.000200    192.168.1.100   192.168.1.200   HTTP     300    GET /malware.php HTTP/1.1
... (all packets will involve one of the malicious IPs)
```
![Expected output](images/exercise-15-output.png)

## Reflection Questions

1.  What are the challenges of performing IOC hunting with a very large list of IP addresses using Wireshark's display filter?
2.  Beyond IP addresses, what other types of Indicators of Compromise (IOCs) could you search for in Wireshark?
3.  How does filtering by malicious IPs help in the early stages of incident response?

## Next Steps

*   [Exercise 16: Encryption Verification](16-encryption-verification.md)
*   Further reading: [Wireshark Display Filter Cheatsheet](https://www.wireshark.org/docs/wsug_html_chunked/ChBuildDisplayFilter.html)

## Hints / Troubleshooting

*   If your malicious IP list is very long, consider using a scripting language (e.g., Python) to automate the generation of the Wireshark filter string.
*   Ensure that the IPs in your `malicious_ips.txt` file are correctly formatted and that there are no extra spaces or characters.
*   If no packets are displayed, it means no communication with those specific IPs occurred in your capture, or the IPs in your list are not present in the PCAP.
