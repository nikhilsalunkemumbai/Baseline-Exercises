---
Title: Exercise 11: Spot Suspicious Connections with TCPView
Objective: Learn to use Sysinternals TCPView to monitor active TCP/UDP connections, identify processes making network connections, and spot suspicious outbound network activity.
Estimated Time: 30-45 minutes
Tools Needed:
*   Sysinternals TCPView (part of Sysinternals Suite) - Download from [Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
Setup Instructions:
1.  Download the Sysinternals Suite and extract it to a convenient location (e.g., `C:\Sysinternals`).
2.  Ensure you have administrative privileges to gain full visibility into all network connections across the system.
3.  (Optional) Open a web browser, email client, or any application that typically makes network connections to generate some baseline traffic.
---

## Steps

1.  **Launch TCPView**
    *   Navigate to your Sysinternals Suite directory (e.g., `C:\Sysinternals`).
    *   Right-click `Tcpview.exe` and select "Run as administrator".
    *   *Observation:* Observe the main TCPView window. It should immediately start populating with active TCP and UDP connections/endpoints. Note the columns: Process, PID, Protocol, Local Address, Local Port, Remote Address, Remote Port, State, Sent Packets, Sent Bytes, Rcvd Packets, Rcvd Bytes.

2.  **Understand the Display and Refresh Rate**
    *   By default, TCPView refreshes every second. You can change this via `Options` -> `Update Speed` or press the spacebar to toggle between automatic and manual refresh.
    *   Observe the color coding:
        *   **Green:** New connections since the last update.
        *   **Red:** Connections that have terminated.
        *   **Yellow:** Connections whose state has changed.
    *   *Action:* Generate some network traffic (e.g., open a new website) and observe the color changes in TCPView.

3.  **Resolve Addresses and Ports**
    *   By default, TCPView attempts to resolve IP addresses to domain names and port numbers to service names (e.g., `80` to `http`, `443` to `https`).
    *   Toggle this option by going to `Options` -> `Resolve Addresses` (or press `Ctrl+R`).
    *   *Question:* How does resolving addresses change your perception of the connections? Which view (resolved or unresolved) might be more useful for different scenarios (e.g., quick overview vs. detailed forensic analysis)?

4.  **Identify Suspicious Connections**
    *   Sort by the "Remote Address" or "Process" column by clicking on their headers.
    *   Look for:
        *   **Unknown processes** making outbound connections.
        *   Connections to **unusual or foreign IP addresses** (especially from processes that shouldn't be communicating externally).
        *   Connections on **non-standard or high-numbered ports** for common applications.
        *   Processes with **many outbound connections** that seem excessive.
        *   Connections in a **suspicious state** (e.g., many `SYN_SENT` without `ESTABLISHED` or `CLOSE_WAIT` for too long).
    *   *Action:* If you see a process you don't recognize making a connection, right-click the row and select "Whois" (if "Resolve Addresses" is enabled) or "Process Properties" to gather more information.

5.  **Terminate a Suspicious Process/Connection (Simulated)**
    *   *Warning:* Do NOT terminate processes unless you are absolutely sure of their nature and impact. This step is for understanding the functionality in a controlled environment.
    *   Right-click on a connection that you *might* deem suspicious (e.g., a browser connection to a known legitimate site after closing the browser tab).
    *   Select "Close Connection" to terminate a specific TCP connection, or "End Process" to terminate the owning process. Observe the effect.
    *   *Reflection:* What are the immediate effects on your system if you close a connection versus ending a process?

## Expected Output

(The actual output will be dynamic based on your system's activity. Below is a description of what you'd typically observe in the GUI.)

```
TCPView graphical user interface displaying a list of network connections.
Columns include: Process, PID, Protocol, Local Address, Local Port, Remote Address, Remote Port, State.
New connections briefly highlighted in green.
Terminated connections briefly highlighted in red.
Resolved domain names (e.g., google.com) and service names (e.g., http) for remote addresses/ports.
```


## Reflection Questions

1.  How can the "State" column in TCPView provide insights into the health or potential compromise of a network connection?
2.  What specific combination of "Process", "Remote Address", and "Remote Port" in TCPView would you prioritize for investigation in a security incident?
3.  Why is running TCPView with administrative privileges crucial for a comprehensive network analysis?

## Next Steps

*   [Exercise 12: Check Loaded DLLs in Process Explorer](../sysinternals/12-check-loaded-dlls.md)
*   Further reading: [TCPView documentation on Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/tcpview)

## Hints / Troubleshooting

*   **Filter out known good traffic:** If your system is very busy, focus on connections from unknown processes or to unusual external IPs. You might need to scroll through a lot of data.
*   **"Show Unconnected Endpoints":** By default, TCPView shows all endpoints. You can deselect "Options" -> "Show Unconnected Endpoints" to declutter the view and focus only on active connections.
*   **Persistent connections:** Some applications maintain persistent connections even when idle. It's important to differentiate these from truly suspicious long-lived connections.
*   **Firewall impact:** Local firewalls might block outbound connections, so "State" might show `SYN_SENT` without `ESTABLISHED` if a connection is blocked.
*   **Whois Lookup:** For external IP addresses, right-clicking and performing a "Whois" lookup can provide ownership information, which helps in assessing the legitimacy of the connection.
