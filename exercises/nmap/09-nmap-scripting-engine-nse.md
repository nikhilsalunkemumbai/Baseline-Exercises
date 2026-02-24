---
Title: Exercise 09: Nmap Scripting Engine (NSE) Basics (--script)
Objective: Learn to use the Nmap Scripting Engine (NSE) to extend Nmap's capabilities beyond simple port scanning, leveraging scripts for tasks like network discovery, vulnerability detection, and deeper service interaction.
Estimated Time: 20-40 minutes
Tools Needed:
*   Nmap (Network Mapper) - Download from [nmap.org](https://nmap.org/download.html)
Setup Instructions:
1.  Ensure Nmap is installed and accessible from your terminal.
2.  Identify a target IP address or hostname. For this exercise, use `scanme.nmap.org` or a target on your local network that you have permission to scan.
---

## Steps

1.  **List Available NSE Scripts**
    *   Nmap comes with a large library of scripts. To list them, you can check the Nmap installation directory or browse online.
    *   On Linux/macOS, scripts are typically in `/usr/share/nmap/scripts/`.
    *   On Windows, they are usually in `C:\Program Files (x86)\Nmap\scripts`.
    *   You can also find them at [nmap.org/nsedoc/](https://nmap.org/nsedoc/).
    *   *Observation:* Familiarize yourself with the categories and names of some scripts.

2.  **Run a Simple Default Script Scan**
    *   Open a terminal.
    *   Run the following command, replacing `[TargetIP]` with your target (e.g., `scanme.nmap.org`):
        ```bash
        nmap -sC [TargetIP]
        ```
    *   *Explanation:* The `-sC` flag is a shortcut for `--script=default`. It runs a set of safe and useful scripts that are categorized as "default". These scripts perform various tasks like gathering more information about services, detecting vulnerabilities, and more.

3.  **Run a Specific Script**
    *   Let's use the `whois-domain` script (requires a domain name, not an IP for best results) or `http-title` (often run by default scripts) for demonstration.
    *   For `scanme.nmap.org`, try the `http-title` script:
        ```bash
        nmap --script http-title [TargetIP]
        ```
    *   *Explanation:* The `--script` flag allows you to specify a single script or a comma-separated list of scripts.

4.  **Run Scripts by Category**
    *   NSE scripts are organized into categories (e.g., `vuln`, `discovery`, `auth`, `brute`).
    *   To run all scripts in the `discovery` category (e.g., to find more hosts or services):
        ```bash
        sudo nmap --script=discovery [TargetIP]
        ```
    *   *Note:* Running scripts can be noisy and may trigger IDS/IPS. Some scripts also require root/admin privileges.

## Expected Output

```
Starting Nmap 7.94 ( https://nmap.org ) at 2024-02-24 12:30 EST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.050s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
|_http-title: Go ahead and ScanMe! <--- Output from http-title script
9929/tcp  open     nping-echo
31337/tcp open     Elite

Nmap done: 1 IP address (1 host up) scanned in 12.10 seconds
```


## Reflection Questions

1.  How does the Nmap Scripting Engine (NSE) enhance the utility of Nmap beyond basic port scanning?
2.  What are some potential risks or ethical considerations when using aggressive or vulnerability-related NSE scripts?
3.  How would you choose which NSE scripts to run for a specific reconnaissance or vulnerability assessment task?

## Next Steps

*   [Exercise 10: Vulnerability Scanning with Nmap NSE](../nmap/10-vulnerability-scanning-nse.md)
*   Further reading: [Nmap Scripting Engine documentation](https://nmap.org/book/nse.html)

## Hints / Troubleshooting

*   **Permissions:** Some scripts (especially those interacting with raw packets or system-level functions) require root/administrator privileges.
*   **Target Selection:** Be cautious when running scripts, especially on production systems. Some scripts can be intrusive or even disruptive.
*   **Script Arguments:** Many scripts accept arguments. You can see script-specific documentation using `nmap --script-help <script-name>`.
*   **Firewalls:** Scripts generate more traffic and are more likely to be detected and blocked by firewalls or IDS.
