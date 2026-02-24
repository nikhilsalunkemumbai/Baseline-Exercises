---
Title: Exercise 03: Reconstruct FTP File
Objective: Learn to reconstruct and save a file that was transferred over unencrypted FTP by following its TCP stream in Wireshark.
Estimated Time: 30-60 minutes
Tools Needed: Wireshark, PCAP file containing unencrypted FTP traffic with a file transfer.
Setup Instructions: 
1. Install Wireshark on your system.
2. Obtain a PCAP file with an unencrypted FTP file transfer. You can:
    *   **Generate your own:** Set up a local FTP server (e.g., FileZilla Server on Windows, `vsftpd` on Linux). Transfer a small, non-sensitive text file (e.g., `test.txt`) from an FTP client to the server (or vice-versa) while capturing traffic with Wireshark on the interface handling that traffic.
    *   **Use a sample:** Search for publicly available PCAP samples that include FTP transfers. *Be cautious when downloading and analyzing PCAPs from unknown sources, especially if they claim to contain malware samples.*
---

## Steps

1.  **Open the PCAP File in Wireshark:**
    *   Launch Wireshark.
    *   Go to `File > Open` and select your PCAP file containing the FTP traffic.

2.  **Filter for FTP Control Channel Traffic:**
    *   In the display filter bar, type `ftp` and press Enter. This will show FTP control commands (e.g., USER, PASS, PORT, RETR, STOR).
    *   Look for packets indicating a file transfer. Typically, this involves `RETR` (retrieve/download) or `STOR` (store/upload) commands. Note the frame number of such a command.

3.  **Identify the Data Channel Connection:**
    *   For the `RETR` or `STOR` command, observe the "Info" column. It usually indicates the port number negotiated for the data transfer (e.g., `PORT 192,168,1,100,195,136` would mean port `195*256 + 136 = 50000`).
    *   Filter for the FTP data channel. If the `RETR` or `STOR` command indicated port `X`, you would filter for `ftp-data` or `tcp.port == X`. If it used a dynamic port, look for `ftp-data` which often runs on `TCP port 20` but can be dynamic. Look for the `227 Entering Passive Mode` (or `PORT` command followed by `150 Opening data channel`) for the specific port.

4.  **Follow the TCP Stream:**
    *   Right-click on an `FTP Data` packet (or a `227 Entering Passive Mode` or `150 Opening data channel` packet that immediately precedes the actual data transfer) related to the file you want to reconstruct.
    *   From the context menu, select `Follow > TCP Stream`.
    *   A new window will open, displaying the raw data exchanged over that TCP stream.

5.  **Save the Reconstructed File:**
    *   In the "Follow TCP Stream" window, you will see the contents of the transferred file.
    *   Ensure "Entire conversation" is selected.
    *   Change "Show data as" to "Raw".
    *   Click the `Save As...` button.
    *   Save the file with its original name and extension (e.g., `my_document.txt`, `image.jpg`). Choose a safe location (e.g., your desktop or a temporary folder).

6.  **Verify the Saved File:**
    *   Open the saved file using an appropriate application (e.g., a text editor for `.txt`, an image viewer for `.jpg`). Confirm its content.

## Expected Output

```
(Expected output will show the contents of the transferred file in the "Follow TCP Stream" window. The saved file should open correctly with its native application.)

Example in "Follow TCP Stream" window (Raw format):
... (HTTP headers or other protocol information might be visible, followed by the file's content)
This is a test file for FTP transfer.
It contains some sample text.
End of file.
...

(After saving and opening the file, its content should match what was transferred.)
```
![Expected output](images/exercise-03-output.png)

## Reflection Questions

1.  What is the significance of the FTP control channel versus the data channel?
2.  Why is it possible to reconstruct the file in Wireshark for FTP, but not typically for HTTPS?
3.  What security risks are associated with transferring sensitive files over unencrypted FTP?

## Next Steps

*   [Exercise 04: Detect Port Scan](04-detect-port-scan.md)
*   Further reading: [Wireshark's "Follow TCP Stream" documentation](https://www.wireshark.org/docs/wsug_html_chunked/ChTransportLayer.html#ChFollowStream)

## Hints / Troubleshooting

*   If `Follow TCP Stream` does not show the file content, ensure you selected a packet from the *data* channel, not just the control channel.
*   Verify that the FTP traffic in your PCAP is indeed unencrypted. Encrypted traffic cannot be reconstructed in this manner.
*   If you see garbled data, ensure you selected "Raw" in the "Show data as" option before saving. Also, verify the file extension you're saving it as matches the actual file type.
