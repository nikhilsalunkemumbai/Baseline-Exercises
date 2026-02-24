---
Title: Outline Eradication and Recovery Steps
Objective: Develop a clear set of eradication and recovery steps for a specific incident scenario, following the guidance of NIST SP 800-61 R2.
Estimated Time: 1 hour
Tools Needed: Text editor, NIST SP 800-61 R2 document
Setup Instructions:
    - Review Section 3.3 "Containment, Eradication, and Recovery" in NIST SP 800-61 R2, specifically focusing on eradication and recovery.
    - Recall the ransomware scenario from Exercise 06.
---

## Steps

1.  **Review NIST Guidance on Eradication and Recovery**
    *   Revisit Section 3.3 "Containment, Eradication, and Recovery" in NIST SP 800-61 R2.
    *   Understand the goals of each phase:
        *   **Eradication**: To remove the cause of the incident (e.g., malware, vulnerabilities).
        *   **Recovery**: To restore affected systems and services to normal operation.

2.  **Recall Incident Scenario: Ransomware Outbreak**
    *   Let's use "Scenario A: Malware Outbreak (Worm/Ransomware)" from Exercise 06 as our basis:
        *   *Description:* A fast-spreading ransomware strain has encrypted files across the network, moving rapidly between systems. Initial containment involved network segmentation and isolating infected hosts.

3.  **Outline Eradication Steps for the Ransomware Scenario**
    *   Based on the ransomware scenario and NIST guidelines, detail the steps you would take to eradicate the threat.
    *   Think about how you would ensure the malware is completely removed and the initial attack vector is closed.
    *   *Consider:* Identification of root cause, removal of malware, patching vulnerabilities.

4.  **Outline Recovery Steps for the Ransomware Scenario**
    *   Following eradication, detail the steps needed to recover systems and data to a pre-incident state.
    *   Think about restoring from backups, verifying integrity, and bringing systems back online securely.
    *   *Consider:* Restoration, testing, hardening, monitoring.

## Expected Output

```markdown
## Eradication and Recovery Steps for Ransomware Outbreak

### Incident Scenario: Ransomware Outbreak
*   **Description:** A fast-spreading ransomware strain has encrypted files across the network, moving rapidly between systems. Initial containment involved network segmentation and isolating infected hosts.

### Eradication Steps

1.  **Identify Root Cause and Initial Access Vector:**
    *   Analyze forensic data (e.g., logs from affected systems, network traffic) to determine how the ransomware initially entered the environment (e.g., phishing email, exploited vulnerability, weak RDP credentials).
    *   Identify all systems that were compromised, not just those encrypted.
    *   Pinpoint the specific strain of ransomware and any associated tools/TTPs used by the attacker.

2.  **Remove Malware and Associated Artifacts:**
    *   For all identified compromised systems:
        *   Wipe and rebuild systems from trusted, golden images (preferred for critical systems).
        *   Alternatively, use anti-malware tools to thoroughly scan and remove all traces of the ransomware and any secondary payloads (e.g., backdoors, credential stealers).
        *   Remove any unauthorized user accounts, scheduled tasks, or services created by the attacker.

3.  **Patch and Harden Identified Vulnerabilities:**
    *   Apply all critical security patches to systems where vulnerabilities were exploited for initial access or lateral movement.
    *   Strengthen security configurations (e.g., disable unused services, restrict RDP access, implement least privilege).
    *   Force password resets for any accounts compromised or suspected of compromise.

### Recovery Steps

1.  **Validate Eradication:**
    *   Perform thorough scans and integrity checks across the network to confirm that all traces of the ransomware and associated threats have been eradicated.
    *   Monitor network traffic for any residual C2 communications.

2.  **Restore from Clean Backups:**
    *   Identify the most recent clean and verified backups of encrypted data and systems.
    *   Restore critical data and systems from these backups to clean, hardened environments.
    *   Prioritize restoration based on business criticality.

3.  **Test Functionality and Security:**
    *   Thoroughly test restored systems and applications to ensure full functionality and data integrity.
    *   Conduct security scans and penetration tests on restored systems before bringing them back online to confirm no residual vulnerabilities or re-infection vectors exist.

4.  **Gradual Reintroduction to Production:**
    *   Slowly and systematically reintroduce systems and network segments back into the production environment, starting with the most critical and thoroughly vetted.
    *   Maintain heightened monitoring during this phase.

5.  **Enhanced Monitoring:**
    *   Implement enhanced monitoring capabilities (e.g., new SIEM correlation rules, EDR alerts) to detect any recurrence of the incident or similar threats.
    *   Monitor for any unusual activity from restored systems.
```


## Reflection Questions

1.  What is the primary difference in goal between "Eradication" and "Recovery"?
2.  Why is it crucial to identify the root cause *before* proceeding with recovery efforts?
3.  How does the availability of good backups significantly impact the recovery process, especially in a ransomware incident?

## Next Steps

*   [Conduct a Post-Incident Review and Identify Lessons Learned](08-post-incident-lessons-learned.md)
*   Further reading: [NIST SP 800-61 R2, Section 3.3](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

## Hints / Troubleshooting

*   **Prioritize:** In a real-world scenario, eradication and recovery steps are prioritized based on business criticality and evidence preservation needs.
*   **Documentation:** Every step should be meticulously documented for future analysis and reporting.
*   **Verify, Verify, Verify:** Do not assume a system is clean or fully functional after a step; always verify.
