---
Title: Develop a Checklist for the Preparation Phase
Objective: Create a comprehensive checklist for the Preparation phase of incident response, based on NIST SP 800-61 R2 guidelines, to ensure an organization is ready before an incident occurs.
Estimated Time: 1 hour
Tools Needed: Text editor or word processor, NIST SP 800-61 R2 document
Setup Instructions:
    - Review Section 3.1 "Preparation" in NIST SP 800-61 R2.
---

## Steps

1.  **Review NIST Preparation Phase Guidance**
    *   Revisit Section 3.1 "Preparation" in NIST SP 800-61 R2.
    *   Pay close attention to the various components and recommendations for establishing an effective incident response capability.
    *   Consider elements like policies, IR team, tools, training, and communication plans.

2.  **Identify Key Preparation Activities**
    *   Based on your review, list all the essential activities and resources an organization should have in place *before* an incident happens. Think broadly across people, processes, and technology.

3.  **Structure the Checklist**
    *   Organize your identified activities into a structured checklist format. You can group related items under sub-headings for clarity (e.g., "Policy & Planning," "Team & Training," "Tools & Infrastructure").
    *   For each item, formulate it as a clear, actionable statement or question that can be checked off.

4.  **Draft the Checklist Items**
    *   Populate your checklist with specific items. Aim for at least 15-20 distinct items.

    *Example Checklist Categories:*
    *   **I. Policy & Planning**
    *   **II. Incident Response Team**
    *   **III. Tools & Environment**
    *   **IV. Training & Awareness**
    *   **V. Communication & Reporting**

## Expected Output

```markdown
## Incident Response Preparation Phase Checklist (NIST SP 800-61 R2 Aligned)

This checklist outlines critical steps an organization should take to prepare for and effectively manage security incidents.

### I. Policy & Planning
*   [ ]  **Incident Response Policy:** Is a formal Incident Response Policy (IRP) documented, approved, and regularly reviewed?
*   [ ]  **IR Plan:** Is a detailed Incident Response Plan (IRP) developed, outlining procedures for each phase of incident response?
*   [ ]  **Legal & Regulatory Compliance:** Are all relevant legal, regulatory, and contractual obligations for incident handling documented and understood?
*   [ ]  **Communication Plan:** Is an incident communication plan in place for internal and external stakeholders?
*   [ ]  **Contact Lists:** Are up-to-date contact lists (internal, external, law enforcement, vendors) maintained and accessible?
*   [ ]  **Business Continuity/Disaster Recovery Integration:** Is the IR plan integrated with existing BC/DR plans?

### II. Incident Response Team
*   [ ]  **Team Structure & Roles:** Is the IR team structure defined with clear roles, responsibilities, and reporting lines?
*   [ ]  **Staffing & Availability:** Is the IR team adequately staffed and available (e.g., 24/7 coverage, on-call rotation)?
*   [ ]  **Skills Assessment:** Are the current skills of the IR team assessed, and are skill gaps identified?

### III. Tools & Environment
*   [ ]  **Security Tools:** Are necessary security tools (e.g., SIEM, EDR, IDS/IPS, vulnerability scanners, forensic tools) deployed and configured?
*   [ ]  **Secure Environment:** Is a secure incident handling environment (e.g., isolated network, forensic workstations) established and maintained?
*   [ ]  **Software/Hardware Inventory:** Is an accurate and up-to-date inventory of critical hardware and software assets maintained?
*   [ ]  **Network Diagrams:** Are current network diagrams and baseline configurations readily available?
*   [ ]  **Logging & Monitoring:** Are appropriate logging and monitoring systems (e.g., central log management, audit logs) implemented across critical systems?
*   [ ]  **Backup & Recovery:** Are regular, tested backups of critical data and systems performed?

### IV. Training & Awareness
*   [ ]  **IR Team Training:** Does the IR team receive regular training on incident response procedures, tools, and emerging threats?
*   [ ]  **Security Awareness Training:** Do all employees receive regular security awareness training, including incident reporting procedures?
*   [ ]  **Tabletop Exercises:** Are periodic tabletop exercises or simulations conducted to test the IR plan and team readiness?

### V. Communication & Reporting
*   [ ]  **Reporting Procedures:** Are clear procedures for internal and external incident reporting established?
*   [ ]  **Public Relations/Legal Support:** Is legal and public relations support identified and available for incident communication?
```


## Reflection Questions

1.  Why is the "Preparation" phase often considered the most crucial phase for effective incident response?
2.  What are the risks of an organization having an incomplete or outdated preparation checklist?
3.  How can conducting regular tabletop exercises improve an organization's readiness for incidents?

## Next Steps

*   [Identify and Analyze Indicators of Compromise (IOCs)](05-detection-analysis-indicators.md)
*   Further reading: [NIST SP 800-61 R2, Section 3.1](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

## Hints / Troubleshooting

*   **Be Specific**: General items like "Have tools" are less useful than "Security Tools: SIEM, EDR, IDS/IPS deployed and configured."
*   **Actionable Items**: Each item should be something that can be checked off as "Done," "In Progress," or "N/A."
*   **Iterative Process**: This checklist is a living document and should be reviewed and updated regularly based on lessons learned and changes in the environment.
