---
Title: Exercise 14: HTTP User-Agents
Objective: Extract and export a list of all HTTP User-Agent strings present in a Wireshark capture, useful for client profiling and identifying unusual browser activity.
Estimated Time: 20-30 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file with HTTP traffic).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Browse to several different HTTP/HTTPS websites. If possible, try using different browsers or even user-agent spoofing extensions to generate varied User-Agent strings. Stop the capture after generating some traffic.
4. **Option B (Existing PCAP):** Use a pre-captured PCAP file that contains HTTP traffic from various clients.
---

## Steps

1.  **Open the PCAP File in Wireshark:**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file.

2.  **Filter for HTTP User-Agent Headers:**
    *   In the display filter bar, type `http.user_agent` and press Enter. This filter will display all HTTP packets that contain a User-Agent header.

3.  **Export Packet Dissections to a File:**
    *   Go to `File > Export Packet Dissections > As CSV...`.

4.  **Configure Export Options:**
    *   In the "Export As CSV" dialog box:
        *   **Packet Range:** Select `Displayed` (to export only packets matching your `http.user_agent` filter).
        *   **Packet Format:** Select `Specific packet data`.
        *   **Fields:** Click the `+` button. In the "Field Name" column, type `http.user_agent`.
        *   **Save as:** Choose a location and filename (e.g., `user_agents.csv`).
    *   Click `Save`.

5.  **Review the Exported File:**
    *   Open the `user_agents.csv` file using a spreadsheet program (like Microsoft Excel, Google Sheets, or LibreOffice Calc) or a text editor.
    *   You should see a list of all User-Agent strings found in the captured HTTP traffic.

## Expected Output

```
(The exported CSV file will contain a column with various HTTP User-Agent strings.)

Example of `user_agents.csv` content:
"http.user_agent"
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"
"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0"
"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Safari/605.1.15"
"Mozilla/5.0 (iPhone; CPU iPhone OS 17_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.0 Mobile/15E148 Safari/604.1"
"curl/7.81.0"
...
```
![Expected output](images/exercise-14-output.png)

## Reflection Questions

1.  What kind of information can be inferred about a client device from its User-Agent string?
2.  How can an analyst use unusual or unexpected User-Agent strings to detect malicious activity on a network?
3.  What are the limitations of relying solely on User-Agent strings for client identification, and how can they be spoofed?

## Next Steps

*   [Exercise 15: IOC Hunting](15-ioc-hunting.md)
*   Further reading: [Wireshark Documentation: Exporting Packet Dissections](https://www.wireshark.org/docs/wsug_html_chunked/ChWorkingExport.html)

## Hints / Troubleshooting

*   If your CSV file is empty, ensure that your Wireshark capture contained HTTP traffic with User-Agent headers.
*   Verify that `http.user_agent` is correctly typed in the filter and the export fields.
*   If you encounter a large number of unique User-Agent strings, you might want to use spreadsheet features (e.g., pivot tables, unique values) to summarize them.
