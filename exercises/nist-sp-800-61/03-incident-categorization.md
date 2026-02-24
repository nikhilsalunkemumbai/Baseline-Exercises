---
Title: Practice Incident Categorization
Objective: Learn to categorize various security incidents based on their severity, impact, and type, following principles aligned with NIST SP 800-61.
Estimated Time: 1 hour
Tools Needed: Text editor, incident categorization matrix (conceptual)
Setup Instructions:
    - Review Section 2.2 "Incident Categories" and Section 2.3 "Prioritization" in NIST SP 800-61 R2.
---

## Steps

1.  **Understand Incident Categories and Prioritization**
    *   Revisit NIST SP 800-61 R2 and review its guidance on incident categories (e.g., unauthorized access, denial of service, malicious code, improper usage) and how to prioritize incidents.
    *   Consider how different incident types might impact confidentiality, integrity, and availability (CIA triad).

2.  **Define a Simple Categorization Matrix**
    *   For this exercise, create a simple categorization matrix based on Severity and Impact.
    *   **Severity Levels (e.g., High, Medium, Low):**
        *   **High:** Immediate and severe impact on critical systems/data, significant financial loss, legal/reputational damage.
        *   **Medium:** Moderate impact, disruption to non-critical services, some data compromise.
        *   **Low:** Minor impact, localized disruption, no significant data loss.
    *   **Impact Areas (e.g., Data, Operations, Reputation, Financial):**
        *   **Data:** Confidentiality, Integrity, Availability of data.
        *   **Operations:** Disruption to business processes.
        *   **Reputation:** Public perception, trust.
        *   **Financial:** Direct costs, lost revenue.

3.  **Analyze Incident Scenarios and Categorize**
    *   Read each of the following incident scenarios.
    *   For each scenario, determine the incident type (using NIST categories or similar), assign a Severity (High, Medium, Low), and briefly justify your categorization based on the potential impact.

    *   **Scenario A:** A highly publicized ransomware attack encrypts all mission-critical servers, rendering them inaccessible. Customer data, including personally identifiable information (PII), is exfiltrated and held for ransom. The attack causes a complete business shutdown for several days.
    *   **Scenario B:** An employee accidentally sends a company-internal confidential document to an external, unauthorized recipient via email. The document contains sensitive project plans but no PII. The employee self-reports the error immediately.
    *   **Scenario C:** A distributed denial-of-service (DDoS) attack temporarily brings down the company's public-facing website for 30 minutes during off-peak hours. No data was compromised, and services were restored quickly.
    *   **Scenario D:** A routine vulnerability scan identifies an unpatched critical vulnerability on an internet-facing server. There is no evidence of exploitation, but the vulnerability could lead to remote code execution.
    *   **Scenario E:** An attacker gains unauthorized access to a development server through a misconfigured SSH service. The attacker creates a new user account but does not access production data or critical systems.

## Expected Output

```markdown
## Incident Categorization Exercise

### Scenario A: Ransomware Attack
*   **Incident Type:** Malicious Code / Unauthorized Access / Denial of Service
*   **Severity:** High
*   **Justification:** Complete business shutdown, exfiltration of PII (data confidentiality/availability loss), significant financial and reputational damage.

### Scenario B: Accidental Data Disclosure
*   **Incident Type:** Improper Usage / Unauthorized Access (Accidental)
*   **Severity:** Medium
*   **Justification:** Breach of confidentiality for sensitive internal data, potential reputational risk, but no PII and prompt self-reporting.

### Scenario C: DDoS Attack
*   **Incident Type:** Denial of Service
*   **Severity:** Medium
*   **Justification:** Temporary disruption to public-facing services (availability loss), but no data compromise and quick restoration during off-peak. Could be High if critical service/peak hours.

### Scenario D: Critical Vulnerability Discovered
*   **Incident Type:** Non-Incident / Event (High-Risk Vulnerability)
*   **Severity:** Low (as an *incident*), but High *risk*
*   **Justification:** No actual compromise has occurred (yet), but the vulnerability poses a significant threat if exploited. This is an event that needs proactive remediation, not a live incident.

### Scenario E: Unauthorized Access to Dev Server
*   **Incident Type:** Unauthorized Access
*   **Severity:** Medium
*   **Justification:** Unauthorized access and user creation on a development server. No production data compromise, but indicates a security lapse and potential for further lateral movement if not contained.
```


## Reflection Questions

1.  How does the "Severity" of an incident differ from its "Impact"?
2.  Why is it important to differentiate between an "event" and an "incident" in security operations?
3.  How can a clear incident categorization matrix help an incident response team prioritize their efforts during a crisis?

## Next Steps

*   [Develop a Checklist for the Preparation Phase](04-preparation-phase-checklists.md)
*   Further reading: [NIST SP 800-61 R2, Section 2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

## Hints / Troubleshooting

*   **Subjectivity**: Incident categorization can sometimes be subjective. Focus on logical reasoning based on potential damage.
*   **Context Matters**: The same event can have different severities depending on the organizational context (e.g., a website defacement is worse for an e-commerce site than for an internal blog).
*   **Initial vs. Evolving Severity**: An incident's severity can change as more information becomes available.
