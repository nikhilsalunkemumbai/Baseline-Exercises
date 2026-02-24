---
Title: Develop an Incident Communication Plan
Objective: Design a structured communication plan for security incidents, identifying key stakeholders, communication channels, and messaging strategies for different incident phases and severity levels, aligned with NIST SP 800-61 R2.
Estimated Time: 1.5 hours
Tools Needed: Text editor or word processor, NIST SP 800-61 R2 document
Setup Instructions:
    - Review Section 3.1 "Preparation" and Section 3.3 "Containment, Eradication, and Recovery" (specifically communication aspects) in NIST SP 800-61 R2.
    - Consider the importance of clear and timely communication during an incident.
---

## Steps

1.  **Review NIST Guidance on Communication**
    *   Revisit NIST SP 800-61 R2, particularly sections discussing communication requirements during incident response.
    *   Note the emphasis on identifying stakeholders, establishing communication channels, and defining escalation procedures.

2.  **Identify Key Stakeholders**
    *   List at least 5-7 distinct groups or individuals who would need to be informed during a significant security incident. Categorize them as internal or external.
    *   *Examples:* Incident Response Team, IT Management, Legal Counsel, Public Relations, Executives, Affected Users/Customers, Law Enforcement, Vendors.

3.  **Define Communication Channels and Frequency**
    *   For each stakeholder group, identify the most appropriate communication channel(s) (e.g., email, dedicated chat, secure phone, press release).
    *   Determine the typical frequency of updates during a critical incident (e.g., hourly, daily, ad-hoc).

4.  **Outline Messaging Strategy per Incident Phase/Severity**
    *   Consider a **Critical Incident** (e.g., major data breach, prolonged system outage). Outline the type of information that needs to be communicated to *internal technical teams*, *executives*, and *external affected customers* during the following phases:
        *   **Initial Detection / Confirmation:** What needs to be said immediately?
        *   **Containment & Analysis:** What updates are crucial as information becomes clearer?
        *   **Eradication & Recovery:** How is progress communicated?
        *   **Resolution & Post-Incident:** What is the final message?

    *   *Self-reflection:* How would the messaging differ for a "Low" severity incident (e.g., a single workstation malware infection) compared to a "Critical" incident?

## Expected Output

```markdown
## Incident Communication Plan Outline

### I. Key Stakeholders

| Stakeholder Group          | Internal/External | Communication Channel(s)      | Typical Update Frequency (Critical Incident) |
| :------------------------- | :---------------- | :---------------------------- | :------------------------------------------- |
| **Incident Response Team** | Internal          | Dedicated Chat (Slack/Teams), Secure Phone, War Room | Continuous / Ad-hoc                          |
| **IT Management**          | Internal          | Email, Secure Phone, Dedicated Chat, Daily Briefings | Hourly (initial), Bi-daily (ongoing)         |
| **Executives/Leadership**  | Internal          | Secure Email, Secure Phone, Dedicated Briefings     | On-demand (initial), Daily Summary           |
| **Legal Counsel**          | Internal          | Secure Email, Secure Phone                        | On-demand / As needed for advice             |
| **Public Relations (PR)**  | Internal          | Secure Email, Secure Phone, Joint Briefings         | On-demand / Prior to external release        |
| **Affected Users/Customers** | External          | Email, Website Banner, Press Release, Customer Support Portal | As dictated by legal/PR/severity             |
| **Law Enforcement**        | External          | Secure Phone, Dedicated Contact (via Legal)         | As dictated by legal/severity                |
| **Third-Party Vendors**    | External          | Secure Email, Phone (relevant contacts)             | As needed for support/coordination           |

### II. Messaging Strategy (for a Critical Incident)

#### A. Internal Technical Teams (IR Team, Network, Systems, Security Operations)
*   **Initial Detection / Confirmation:**
    *   "Critical incident confirmed: [Brief description - e.g., 'Widespread ransomware infection']. All hands on deck. Join Incident Bridge: [Link/Number]. Refer to IR Playbook section [X]."
*   **Containment & Analysis:**
    *   "Update: [X]% of systems contained in network segment [Y]. Initial analysis points to [Z] as initial vector. Prioritizing forensic imaging of [A]. Next update in [X] mins."
*   **Eradication & Recovery:**
    *   "Update: Malware eradication commenced on [X] systems. Restoring from backups for [Y] critical service. Estimated time to full recovery: [Z] hours. Continued monitoring essential."
*   **Resolution & Post-Incident:**
    *   "Incident resolved: All affected systems restored, threat eradicated. Post-incident review scheduled for [Date/Time]. Maintain heightened vigilance."

#### B. Executives/Leadership
*   **Initial Detection / Confirmation:**
    *   "Urgent: We have confirmed a significant cybersecurity incident affecting [brief scope, e.g., 'critical business operations']. IR team is fully engaged in containment. Initial impact assessment underway. Will provide an update within [X] minutes/hours."
*   **Containment & Analysis:**
    *   "Update on incident: Containment efforts are progressing, [X]% of critical systems isolated. Initial assessment suggests [potential impact/data type]. No confirmed data loss/exfiltration at this time. Legal and PR advised. Next update: [Time]."
*   **Eradication & Recovery:**
    *   "Incident update: Threat eradicated from [X] systems. Recovery of [Y] services initiated. Expecting [Z] service restoration by [Time/Date]. Continuous monitoring in place. Legal/PR readiness in progress."
*   **Resolution & Post-Incident:**
    *   "Incident resolved: All services restored, threat neutralized. Investigations ongoing to confirm full eradication and identify root cause. Comprehensive post-incident review to follow. We appreciate your support and confidence."

#### C. External Affected Customers
*   **Initial Detection / Confirmation:**
    *   (Typically, no external communication until more is known, often in consultation with Legal/PR). If critical service outage: "Dear customers, we are experiencing an unplanned service interruption with [Service Name]. Our teams are actively investigating and working to restore services. We apologize for any inconvenience." (Via website banner/status page).
*   **Containment & Analysis:**
    *   "Update on service interruption: We have identified the cause of the issue and are implementing measures to restore full service. We have no evidence that customer data has been compromised. Further updates will be provided."
*   **Eradication & Recovery:**
    *   "Service Restoration: [Service Name] has been fully restored and is operating normally. We continue to monitor the situation closely. We will provide a full post-incident report outlining the cause and preventative measures."
*   **Resolution & Post-Incident:**
    *   "Post-Incident Report: [Link to statement/FAQ explaining the incident, actions taken, and what customers can expect]. We value your trust and are committed to maintaining the security of our services."
```
![Expected output](images/exercise-09-output.png)

## Reflection Questions

1.  Why is a tailored communication strategy essential for different stakeholder groups during an incident?
2.  What are the risks of poor or untimely communication during a critical security incident?
3.  How can having pre-approved message templates help expedite communication during an incident?

## Next Steps

*   [Understand Best Practices for Forensic Data Collection](10-forensic-data-collection.md)
*   Further reading: [NIST SP 800-61 R2, Section 3.1.2 (Incident Response Policy and Plan)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

## Hints / Troubleshooting

*   **Be Proactive**: Pre-planning communication avoids panic and ensures a consistent message.
*   **Legal/PR Review**: All external communications, especially for critical incidents, should be reviewed by legal counsel and public relations.
*   **Assume Worst Case**: Plan for the most severe incidents to ensure your communication strategy holds up under pressure.
