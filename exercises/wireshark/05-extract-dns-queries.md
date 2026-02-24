---
Title: Exercise 05: Extract DNS Queries
Objective: Learn to filter Wireshark traffic for DNS queries and then extract a list of all unique domain names that were queried.
Estimated Time: 20-30 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Browse several websites (e.g., Google, Wikipedia, your favorite news site) to generate DNS traffic, then stop the capture.
4. **Option B (Existing PCAP):** Use a pre-captured PCAP file that is known to contain DNS traffic.
---

## Steps

1.  **Open the PCAP File in Wireshark (or use Live Capture):**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file, or start a live capture (as described in Setup).

2.  **Filter for DNS Traffic:**
    *   In the display filter bar, type `dns` and press Enter. This will show all DNS-related packets.

3.  **Further Filter for DNS Queries:**
    *   To focus specifically on queries, refine the display filter to `dns.flags.response == 0`. This filters for packets where the DNS response flag is not set, indicating a query.

4.  **Extract Queried Domain Names:**
    *   Go to `Statistics > Resolved Addresses`.
    *   In the "Resolved Addresses" window, you should see a list of IP addresses that Wireshark resolved during the capture. This list implicitly contains the domain names that were queried.
    *   Alternatively, to get a more direct list of query names:
        *   Go to `Statistics > Endpoints`.
        *   Select the "IPv4" or "IPv6" tab.
        *   Look for IP addresses that correspond to DNS servers (e.g., 8.8.8.8, 1.1.1.1, or your local router's IP).
        *   A more direct way to list the queried domains: go to `File > Export Packet Dissections > As Plain Text...`.
        *   In the "Export As Plain Text" dialog, select "Displayed" for packets.
        *   For "Packet Format", choose "Summary line, current columns".
        *   Save it as a `.txt` file.
        *   Open the `.txt` file and search for "Standard query" to find the domain names.

5.  **Using `File > Export Packet Dissections > As CSV...` for detailed extraction:**
    *   Apply the `dns.qry.name` filter. This filter shows only packets that contain a DNS query name.
    *   Go to `File > Export Packet Dissections > As CSV...`.
    *   In the dialog, select "Displayed" packets.
    *   For "Packet Format", choose "Specific packet data".
    *   In the "Fields" area, add `dns.qry.name`.
    *   Save as `dns_queries.csv`.
    *   Open the CSV file in a spreadsheet program to easily list all queried domain names.

## Expected Output

```
(Expected output involves filtering the packet list to show DNS queries. The "Statistics > Resolved Addresses" window or an exported text/CSV file will contain the list of queried domain names.)

Example in Wireshark Packet List (after applying `dns.flags.response == 0`):
No.     Time        Source          Destination     Protocol Length Info
10      0.000123    192.168.1.100   8.8.8.8         DNS      75     Standard query A www.google.com
15      0.000456    192.168.1.100   1.1.1.1         DNS      78     Standard query A wikipedia.org
...

Example from exported `dns_queries.csv`:
"dns.qry.name"
"www.google.com"
"wikipedia.org"
"example.com"
"api.thirdparty.com"
...
```


## Reflection Questions

1.  Why is it important to specifically filter for `dns.flags.response == 0` when trying to list queried domain names, rather than just `dns`?
2.  How can analyzing DNS queries help in detecting suspicious activity on a network?
3.  What challenges might arise when trying to extract DNS queries from a very large PCAP file?

## Next Steps

*   [Exercise 06: Follow TCP Stream](06-follow-tcp-stream.md)
*   Further reading: [Wireshark Display Filter Reference: DNS](https://www.wireshark.org/docs/dfref/d/dns.html)

## Hints / Troubleshooting

*   If you don't see any DNS traffic, ensure your network activity generated DNS requests (e.g., browsing new websites, pinging hostnames).
*   Check that your capture interface was correctly selected.
*   If using `Export Packet Dissections`, ensure you select the appropriate "Packet Format" and "Fields" to get the desired output.
