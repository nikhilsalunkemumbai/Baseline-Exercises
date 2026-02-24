---
Title: Exercise 11: IO Graph
Objective: Use Wireshark's I/O Graph feature to visualize network traffic rates (packets/bytes per second) over time and identify unusual spikes or patterns that might indicate anomalies.
Estimated Time: 20-30 minutes
Tools Needed: Wireshark, active network connection (or a pre-captured PCAP file with varied traffic).
Setup Instructions: 
1. Install Wireshark on your system.
2. Ensure you have an active internet connection.
3. **Option A (Live Capture):** Start a Wireshark capture on your active network interface. Generate varied network traffic for a few minutes (e.g., download a large file, stream a short video, browse several websites, or even perform a brief ping flood to a local host to create a noticeable spike). Stop the capture.
4. **Option B (Existing PCAP):** Use a pre-captured PCAP file that contains varying levels of network traffic.
---

## Steps

1.  **Open the PCAP File in Wireshark (or use Live Capture):**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file, or start and stop a live capture as described in Setup.

2.  **Open the I/O Graph Window:**
    *   Go to `Statistics > I/O Graphs`. A new window titled "I/O Graph" will open, displaying a graph of packet activity over time.

3.  **Analyze the Default Graph:**
    *   By default, the graph shows "All packets" (typically a green line) plotted against time.
    *   Observe the patterns. Look for any sudden peaks (spikes) or troughs (drops) in traffic. These can indicate significant events, such as a large file transfer, a network attack, or a period of inactivity.

4.  **Add Custom Filters to the Graph (to spot anomalies):**
    *   In the I/O Graph window, at the bottom, there is an "Enabled Graphs" section.
    *   Click the `+` button next to "Filter" for an empty graph slot (e.g., "Graph 1").
    *   **Graph 1:**
        *   **Filter:** Type `http` (to see HTTP traffic).
        *   **Color:** Choose a distinct color (e.g., Red).
        *   **Style:** Choose "Line" or "Impulse".
    *   **Graph 2:**
        *   **Filter:** Type `tcp.analysis.retransmission` (to spot retransmissions, often indicating packet loss).
        *   **Color:** Choose another distinct color (e.g., Blue).
        *   **Style:** Choose "Line" or "Impulse".
    *   Click the "Add" button for each filter. The graph will update to show these new lines.

5.  **Adjust Graph Settings (Optional):**
    *   **Interval:** Change the "Interval" (e.g., to `0.01 sec`, `1 sec`, or `10 sec`) to zoom in or out on time segments and reveal finer details or broader trends.
    *   **Y Axis:** Change the "Y Axis" dropdown (e.g., to `Bytes/tick`, `Bits/tick`) to view traffic in terms of data volume instead of packet count.

6.  **Interpret Anomalies:**
    *   Correlate spikes in "All packets" with your custom filters. For example, a spike in "All packets" coinciding with a spike in `tcp.analysis.retransmission` could indicate network congestion or a problem.
    *   Spikes in `http` traffic might correspond to browsing activity or a large download.

## Expected Output

```
(The I/O Graph window will display a visual representation of network traffic over time. You will see lines representing different filters (e.g., all packets, HTTP traffic, TCP retransmissions) and any spikes or drops in these lines.)

Example: (Imagine a graph with a green line for "All packets" showing general activity. A red line for "HTTP" might show a few small bumps. A blue line for "TCP Retransmissions" might show a flat line, or a small spike during a period of network issue.)

[Wireshark I/O Graph window showing X-axis as "Time (s)" and Y-axis as "Packets/tick". Multiple colored lines representing different filters (e.g., "All packets", "http", "tcp.analysis.retransmission") are plotted. Peaks and troughs in these lines are visible.]
```


## Reflection Questions

1.  How can an I/O Graph help differentiate between normal network activity and a potential Denial-of-Service (DoS) attack?
2.  What kind of anomaly would a sustained, high baseline in the "All packets" graph (without corresponding application-level traffic) suggest?
3.  How can changing the graph interval help in identifying different types of network anomalies (e.g., short bursts vs. long-term trends)?

## Next Steps

*   [Exercise 12: Top Talkers](12-top-talkers.md)
*   Further reading: [Wireshark Wiki: I/O Graph](https://wiki.wireshark.org/IO_Graphs)

## Hints / Troubleshooting

*   If your graph is mostly flat, ensure you generated enough traffic during your capture.
*   Make sure your filters are correctly typed. Invalid filters will not produce a graph line.
*   Experiment with different Y-axis units (e.g., `Packets/tick`, `Bytes/tick`, `Bits/tick`) and intervals to get the most informative view of your data.
