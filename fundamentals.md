\## 16 Bite‑Size Ideas per Tool (Baseline Focus)



Each idea is a self-contained module, achievable in 1–2 hours with minimal setup, designed to build demonstrable expertise.



---



\### \*\*Wireshark\*\* (Networking)



1\. \*\*Capture HTTP Request\*\* – Start a capture, visit a website, stop, and filter to `http` to see the request and response.

2\. \*\*Filter by IP\*\* – Isolate all traffic to/from a specific IP using `ip.addr == x.x.x.x`.

3\. \*\*Reconstruct FTP File\*\* – From a pcap with FTP traffic, follow the TCP stream and save the transferred file.

4\. \*\*Detect Port Scan\*\* – Analyse a capture containing a port scan (use a sample pcap) by looking for many sequential connection attempts.

5\. \*\*Extract DNS Queries\*\* – Filter `dns` and list all queried domain names.

6\. \*\*Follow TCP Stream\*\* – Pick an HTTP stream and follow it to see the full conversation.

7\. \*\*TLS Handshake Inspection\*\* – Filter `tls.handshake.type == 1` and examine the Client Hello for SNI and cipher suites.

8\. \*\*MAC Address Filter\*\* – Use `eth.addr == aa:bb:cc:dd:ee:ff` to see all traffic from a specific device.

9\. \*\*Custom TCP Flags Column\*\* – Add a column for `tcp.flags` to quickly spot SYN, ACK, etc.

10\. \*\*Detect ARP Spoofing\*\* – Look for multiple different MAC addresses claiming the same IP in ARP packets.

11\. \*\*IO Graph\*\* – Create an IO Graph to visualise traffic rate and spot anomalies.

12\. \*\*Top Talkers\*\* – Use Statistics → Endpoints to see which IPs sent the most packets.

13\. \*\*Rogue DHCP Server\*\* – Filter `dhcp` and look for multiple DHCP offer packets from different MACs.

14\. \*\*HTTP User‑Agents\*\* – Export all HTTP User‑Agent strings using a display filter and the “Export Packet Dissections” feature.

15\. \*\*IOC Hunting\*\* – Given a list of malicious IPs, filter traffic to see if any communication occurred.

16\. \*\*Encryption Verification\*\* – Check that a HTTPS connection uses TLS by looking for absence of plaintext HTTP and valid certificate details.



---



\### \*\*Sysinternals Suite\*\* (Windows OS Internals)



1\. \*\*Find Fake svchost\*\* – Use Process Explorer to check the path of every `svchost.exe`; the real ones are in `C:\\Windows\\System32`.

2\. \*\*Disable Malware Auto‑start\*\* – Use Autoruns to locate an suspicious entry and disable it.

3\. \*\*File Lock Detective\*\* – Use Handle to find which process is locking a specific file (e.g., `handle -a "filename"`).

4\. \*\*Monitor Process Activity\*\* – Use Process Monitor to log registry and file activity of a calculator process.

5\. \*\*Remote Command\*\* – Use PsExec to run `ipconfig` on a remote machine (requires admin credentials).

6\. \*\*Verify Signatures\*\* – Use Sigcheck to check system files: `sigcheck -s C:\\Windows\\System32`.

7\. \*\*Check Service Permissions\*\* – Use AccessChk to see who can modify a service: `accesschk -c "service name"`.

8\. \*\*Alternate Data Streams\*\* – Use Streams to check if a file has hidden ADS: `streams file.txt`.

9\. \*\*Run as Another User\*\* – Use ShellRunas to launch a program with different credentials.

10\. \*\*Analyse Memory Usage\*\* – Use VMMap to see the heap, stack, and virtual allocations of a process.

11\. \*\*Spot Suspicious Connections\*\* – Use TCPView to look for unknown processes making outbound connections.

12\. \*\*Check Loaded DLLs\*\* – In Process Explorer, view the DLL list of a process and sort by “Company Name” to find unsigned/unverified DLLs.

13\. \*\*Baseline Autoruns\*\* – Run Autoruns on a clean Windows install and save the configuration for later comparison.

14\. \*\*Capture a Registry Change\*\* – Use Process Monitor with a filter to capture exactly when a registry key is modified.

15\. \*\*Gather Remote System Info\*\* – Use PsInfo to get detailed info from a remote PC: `psinfo \\\\computer`.

16\. \*\*Monitor Disk for Ransomware\*\* – Use DiskMon to log all disk writes and look for high‑frequency renaming events.



---



\### \*\*Nmap\*\* (Security Fundamentals)



1\. \*\*Discover Live Hosts\*\* – `nmap -sn 192.168.1.0/24` to ping sweep a subnet.

2\. \*\*TCP SYN Scan\*\* – `nmap -sS 192.168.1.10` to see open ports (stealth scan).

3\. \*\*Service Version Detection\*\* – `nmap -sV 192.168.1.10` to identify running services and versions.

4\. \*\*OS Fingerprinting\*\* – `nmap -O 192.168.1.10` to guess the operating system.

5\. \*\*Vulnerability Script\*\* – `nmap --script smb-vuln-ms17-010 -p445 192.168.1.10` to check for EternalBlue.

6\. \*\*UDP Scan\*\* – `nmap -sU 192.168.1.10` to find open UDP ports (can be slow; try top 100 ports).

7\. \*\*Ping Sweep Only\*\* – `nmap -sP 192.168.1.0/24` (same as `-sn`) to list live hosts.

8\. \*\*Firewall Detection\*\* – `nmap -sA 192.168.1.10` to see if packets are filtered (ACK scan).

9\. \*\*Port Range and Grepable Output\*\* – `nmap -p 1-1000 -oG - 192.168.1.10` for easy parsing.

10\. \*\*Enumerate SMB Shares\*\* – `nmap --script smb-enum-shares -p445 192.168.1.10`.

11\. \*\*Spoof Source IP\*\* – `nmap -S 10.0.0.1 -e eth0 192.168.1.10` (requires root and proper network).

12\. \*\*SSL/TLS Cipher Enumeration\*\* – `nmap --script ssl-enum-ciphers -p 443 192.168.1.10`.

13\. \*\*Decoy Scan\*\* – `nmap -D RND:5 192.168.1.10` to obscure your source IP.

14\. \*\*SNMP Info\*\* – `nmap -sU -p 161 --script snmp-info 192.168.1.10`.

15\. \*\*Scan Comparison\*\* – Save two scan outputs (`-oA before`, `-oA after`) and use `ndiff` to see changes.

16\. \*\*Traceroute\*\* – `nmap --traceroute 192.168.1.10` to see the path packets take.



---



\### \*\*Python\*\* (Scripting / Automation)



1\. \*\*Ping Sweep\*\* – Write a script that pings a list of IPs and reports live hosts using `subprocess` or `ping3`.

2\. \*\*Parse Windows Event Logs\*\* – Use `evtx` library to extract failed logins (Event ID 4625) from an `.evtx` file.

3\. \*\*VirusTotal Hash Checker\*\* – Script that reads a file of hashes and queries the VirusTotal API (public key) to retrieve detection ratios.

4\. \*\*Apache Log Parser\*\* – Parse an access log, extract all unique IPs, and count requests per IP.

5\. \*\*Convert Zeek Log to CSV\*\* – Read a Zeek `conn.log` and output selected fields as CSV.

6\. \*\*Download and Hash\*\* – Script to download a file from a URL and compute SHA256.

7\. \*\*Directory Monitor\*\* – Watch a folder for new files and print a message when one appears (using `watchdog`).

8\. \*\*Shodan Query\*\* – Use Shodan API to get open ports for a target IP and print them.

9\. \*\*Automate Nmap and Parse\*\* – Run an Nmap scan from Python, capture XML output, and parse it with `lxml` to list open ports.

10\. \*\*Log Timeline Generator\*\* – Read multiple log files (e.g., Apache, Syslog) and merge them into a single timeline with timestamps.

11\. \*\*SSL Certificate Expiry Checker\*\* – For a list of domains, connect via SSL and print days until expiry.

12\. \*\*Extract Email Addresses\*\* – Use regex to find all email addresses in a text file.

13\. \*\*Base64 Encoder/Decoder\*\* – Script that accepts input and encodes/decodes base64 (useful for decoding malware strings).

14\. \*\*Active Directory User Enumeration\*\* – Use `ldap3` to connect to an AD controller and list all users.

15\. \*\*Parse PCAP with Scapy\*\* – Read a pcap file and list all unique source IPs.

16\. \*\*Process Alerting\*\* – Script that monitors running processes (using `psutil`) and sends a Slack message if a specific process name appears.



---



\### \*\*Elastic Stack (ELK)\*\* (Logging \& Monitoring)



1\. \*\*Single‑Node Setup\*\* – Install Elasticsearch and Kibana on a local machine (using Docker or packages) and confirm they run.

2\. \*\*Index a Sample Log\*\* – Ingest a sample Apache log file using Filebeat or the Kibana “Upload file” feature.

3\. \*\*Basic Dashboard\*\* – Create a dashboard in Kibana showing a line chart of requests over time.

4\. \*\*HTTP Status Codes Pie Chart\*\* – Use Kibana Lens to create a pie chart of response codes from Apache logs.

5\. \*\*Search by IP\*\* – Use Kibana Discover to find all events from a specific IP address using a Lucene query.

6\. \*\*Top Source IPs\*\* – Build a visualisation (table) of the most frequent source IPs.

7\. \*\*Simple Alert\*\* – Set up an Elastic alert (Stack Monitoring or Watcher) to email when an index exceeds a certain size.

8\. \*\*Ingest Windows Event Logs\*\* – Configure Winlogbeat to forward Security events to Elasticsearch.

9\. \*\*Custom Log Parsing with Logstash\*\* – Use a Grok filter to parse a custom log format and output to Elasticsearch.

10\. \*\*Timelion Graph\*\* – Use Timelion to plot network traffic data over time (e.g., `.es(index=winlogbeat\*, metric=avg:event.duration)`).

11\. \*\*Geolocation Map\*\* – Create a map visualisation in Kibana by using an IP‑to‑location ingest pipeline.

12\. \*\*SIEM Detection\*\* – Enable Elastic SIEM and use a prebuilt rule to detect Mimikatz usage (if relevant logs exist).

13\. \*\*Failed SSH Dashboard\*\* – Build a dashboard visualising failed SSH attempts from `/var/log/auth.log` (or Windows Event 4625).

14\. \*\*Filter and Export\*\* – In Discover, filter events for a specific date range and export the results as CSV.

15\. \*\*Runtime Field\*\* – Create a runtime field in an index pattern to compute a new field (e.g., `source.geo.city\_name`).

16\. \*\*Anomaly Detection\*\* – Use the Machine Learning “Single Metric Job” to detect unusual traffic spikes (requires Platinum license, but can be explored with trial).



---



\### \*\*NIST SP 800‑61\*\* (Legal \& Regulatory Framework)



1\. \*\*Summarise IR Phases\*\* – Write a one‑page summary of the four phases: Preparation, Detection \& Analysis, Containment/Eradication/Recovery, Post‑Incident.

2\. \*\*Preparation Checklist\*\* – Create a checklist of items an organisation should have ready before an incident (policies, tools, contacts).

3\. \*\*Sample IR Policy\*\* – Draft a concise incident response policy based on NIST guidelines.

4\. \*\*Communication Plan\*\* – Outline a plan listing who to contact internally (legal, PR, executives) and externally (law enforcement, CERT) during an incident.

5\. \*\*Incident Report Template\*\* – Design a template with sections for executive summary, timeline, findings, and lessons learned.

6\. \*\*Legal Considerations\*\* – List key legal steps: chain of custody, privacy laws, evidence handling, and reporting obligations.

7\. \*\*Post‑Incident Review Steps\*\* – Write a step‑by‑step guide for conducting a lessons‑learned meeting.

8\. \*\*Map NIST to Ransomware\*\* – Take a ransomware scenario and map actions to each NIST phase.

9\. \*\*Tabletop Exercise Scenario\*\* – Develop a simple scenario (e.g., phishing email leads to compromise) and facilitator questions.

10\. \*\*Definitions Cheat Sheet\*\* – Create a quick reference for terms: event, incident, alert, indicator, etc.

11\. \*\*Evidence Preservation Procedure\*\* – Write a one‑page procedure for collecting and preserving digital evidence (order of volatility, hashing).

12\. \*\*Compare with SANS PICERL\*\* – List the phases of NIST 800‑61 alongside SANS PICERL and note differences.

13\. \*\*Internal Roles Memo\*\* – Draft a memo to staff explaining their responsibilities during an incident.

14\. \*\*Containment Strategies\*\* – List possible containment actions (isolate host, block IP, disable account) with brief descriptions.

15\. \*\*External Coordination\*\* – Outline steps to coordinate with law enforcement and other external parties.

16\. \*\*Incident Declaration Form\*\* – Create a one‑page form to formally declare an incident (date, reporter, initial impact, etc.).



---



Each of these ideas is a standalone exercise that can be completed in a short session, producing a tangible output (script, report, configuration, guide) that demonstrates your growing expertise. They are designed to be practical, directly applicable to Baseline work, and easy to add to your portfolio.

