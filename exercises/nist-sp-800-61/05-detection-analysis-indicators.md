---
Title: Identify and Analyze Indicators of Compromise (IOCs)
Objective: Learn to identify and analyze common Indicators of Compromise (IOCs) and incident symptoms, crucial for the Detection and Analysis phase of incident response.
Estimated Time: 1 hour
Tools Needed: Text editor, web browser for research
Setup Instructions:
    - Review Section 3.2 "Detection and Analysis" in NIST SP 800-61 R2.
    - Understand the concept of an Indicator of Compromise (IOC).
---

## Steps

1.  **Review NIST Guidance on Detection and Analysis**
    *   Revisit Section 3.2 "Detection and Analysis" in NIST SP 800-61 R2.
    *   Pay attention to how incidents are detected, analyzed, and categorized. Focus on the types of symptoms and indicators that suggest an incident is occurring or has occurred.

2.  **Define Indicator of Compromise (IOC)**
    *   In your own words, define what an Indicator of Compromise (IOC) is in the context of cybersecurity. Provide a few examples of different types of IOCs (e.g., IP address, hash, domain).

3.  **Analyze Incident Symptoms and Map to IOCs**
    *   Consider the following common incident symptoms. For each symptom, identify potential underlying causes and list specific IOCs you would look for to confirm or investigate the issue.

    *   **Symptom 1: Unusual Outbound Network Traffic**
        *   *Potential Causes:* Data exfiltration, command and control (C2) communication, botnet activity.
        *   *IOCs to look for:*
            *   Unusual destination IP addresses/domains (e.g., known bad IPs, unapproved cloud storage).
            *   High volume of data to external hosts.
            *   Uncommon protocols or ports used for outbound communication.
            *   Traffic to blacklisted countries.

    *   **Symptom 2: Multiple Failed Login Attempts from a Single Source**
        *   *Potential Causes:* Brute-force attack, password spraying.
        *   *IOCs to look for:*
            *   Source IP address.
            *   Targeted username(s).
            *   Timestamp and frequency of attempts.

    *   **Symptom 3: Unexpected System Reboots or Crashes**
        *   *Potential Causes:* Malicious software (e.g., rootkits, wipers), operating system exploits, hardware failure.
        *   *IOCs to look for:*
            *   Event logs (crash dumps, system errors).
            *   New/modified drivers or kernel modules.
            *   Unusual processes or services.

    *   **Symptom 4: Unauthorized Changes to System Files or Configurations**
        *   *Potential Causes:* Malware, insider threat, unauthorized access.
        *   *IOCs to look for:*
            *   Hashes of system files (comparison against known good).
            *   Registry modifications.
            *   New or modified scheduled tasks.
            *   Changes to user accounts/permissions.

    *   **Symptom 5: Discovery of Unfamiliar Files or Programs**
        *   *Potential Causes:* Malware, unauthorized software installation.
        *   *IOCs to look for:*
            *   File names and paths.
            *   File hashes (MD5, SHA256).
            *   File creation/modification dates.
            *   Digital signatures (missing or invalid).

4.  **Reflect on Data Sources for IOCs**
    *   For each type of IOC (IP, hash, domain, process name, etc.), consider what log sources or security tools would typically provide this information (e.g., firewall logs, SIEM, EDR, network traffic capture).

## Expected Output

```markdown
## Identification and Analysis of Indicators of Compromise (IOCs)

### Definition of Indicator of Compromise (IOC)
An Indicator of Compromise (IOC) is a piece of forensic data, such as data found in system logs or files, that identifies potentially malicious activity on a system or network. IOCs are artifacts observed on a network or operating system that reliably indicate a computer intrusion. Examples include IP addresses, file hashes, domain names, and registry keys.

### Analysis of Incident Symptoms and Associated IOCs

1.  **Symptom: Unusual Outbound Network Traffic**
    *   **Potential Causes:** Data exfiltration, command and control (C2) communication, botnet activity.
    *   **IOCs to look for:**
        *   Destination IP addresses / domain names (e.g., known malicious, unapproved services).
        *   Unusual data volume/patterns to external hosts.
        *   Source/Destination Ports and Protocols (e.g., uncommon ports for specific applications).
        *   User Agent strings for HTTP traffic.

2.  **Symptom: Multiple Failed Login Attempts from a Single Source**
    *   **Potential Causes:** Brute-force attack, password spraying, account lockout attempts.
    *   **IOCs to look for:**
        *   Source IP address of failed attempts.
        *   Targeted username(s).
        *   Timestamp and frequency of authentication failures.
        *   Authentication logs (e.g., Windows Event ID 4625).

3.  **Symptom: Unexpected System Reboots or Crashes**
    *   **Potential Causes:** Malicious software (e.g., rootkits, kernel exploits), operating system vulnerabilities, hardware instability.
    *   **IOCs to look for:**
        *   System Event Logs (e.g., Windows Event IDs for critical errors, unexpected shutdowns).
        *   Crash dump files (analysis for root cause).
        *   New or modified drivers/kernel modules.
        *   Unusual processes running at startup or with high privileges.

4.  **Symptom: Unauthorized Changes to System Files or Configurations**
    *   **Potential Causes:** Malware infection, insider threat, privilege escalation.
    *   **IOCs to look for:**
        *   Hashes of critical system files that differ from baselines.
        *   Modifications to registry keys (e.g., Run keys, security policies).
        *   Creation/modification of new user accounts or changes to existing permissions.
        *   New or altered scheduled tasks.

5.  **Symptom: Discovery of Unfamiliar Files or Programs**
    *   **Potential Causes:** Malware droppers, unauthorized software, advanced persistent threats (APTs).
    *   **IOCs to look for:**
        *   File names and directory paths (especially in unusual locations).
        *   File hashes (MD5, SHA256) for identification against threat intelligence.
        *   File creation/modification/access timestamps.
        *   Digital signatures (absence of, or invalid signature).
        *   Process names/command-line arguments.
```


## Reflection Questions

1.  How do IOCs assist in the early detection and rapid identification of security incidents?
2.  What is the importance of having multiple types of IOCs when investigating an incident?
3.  How does timely analysis of incident symptoms and IOCs contribute to minimizing the impact of a security breach?

## Next Steps

*   [Evaluate and Apply Containment Strategies](06-containment-strategies.md)
*   Further reading: [NIST SP 800-61 R2, Section 3.2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
*   Further reading: [MITRE ATT&CK Framework](https://attack.mitre.org/) (for mapping IOCs to TTPs)

## Hints / Troubleshooting

*   **Context is Key**: An IOC alone may not confirm an incident; context from other logs and systems is vital.
*   **False Positives**: Be aware that some IOCs might be benign in certain contexts (e.g., a new legitimate software update).
*   **Evolving Threats**: Threat actors constantly change their tactics, techniques, and procedures (TTPs), so IOCs need to be regularly updated.
