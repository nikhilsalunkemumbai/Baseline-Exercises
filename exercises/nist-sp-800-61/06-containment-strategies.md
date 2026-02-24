---
Title: Evaluate and Apply Containment Strategies
Objective: Understand and differentiate various containment strategies for security incidents, and apply the most appropriate approach based on incident type and organizational context, guided by NIST SP 800-61 R2.
Estimated Time: 1 hour
Tools Needed: Text editor, NIST SP 800-61 R2 document
Setup Instructions:
    - Review Section 3.3 "Containment, Eradication, and Recovery" in NIST SP 800-61 R2, specifically focusing on containment strategies.
---

## Steps

1.  **Review NIST Guidance on Containment**
    *   Revisit Section 3.3 "Containment, Eradication, and Recovery" in NIST SP 800-61 R2.
    *   Focus on the different containment strategies discussed (e.g., short-term, long-term, segmenting networks, disabling functions).
    *   Understand the goals of containment: to limit the scope and impact of an incident.

2.  **Identify Key Factors Influencing Containment Strategy**
    *   Based on NIST guidelines and general incident response best practices, list the critical factors an incident responder must consider when deciding on a containment strategy.
    *   *Hint:* Think about impact, resources, evidence preservation, and business continuity.

3.  **Analyze Incident Scenarios and Propose Containment**
    *   For each of the following incident scenarios, propose a primary containment strategy or a sequence of steps, and justify your choice based on the incident characteristics and your identified factors.

    *   **Scenario A: Malware Outbreak (Worm/Ransomware)**
        *   *Description:* A fast-spreading ransomware strain is encrypting files across the network, moving rapidly between systems.
        *   *Factors to consider:* Speed of spread, potential for data destruction, need to preserve evidence, impact on operations.
        *   *Proposed Containment:* ...
        *   *Justification:* ...

    *   **Scenario B: Data Exfiltration (Slow & Stealthy)**
        *   *Description:* Suspicious outbound traffic to an unknown external IP address indicates a slow, persistent data exfiltration from a critical database server.
        *   *Factors to consider:* Need for covert monitoring, evidence preservation, minimizing business disruption, identifying all affected data.
        *   *Proposed Containment:* ...
        *   *Justification:* ...

    *   **Scenario C: Web Server Compromise (Defacement)**
        *   *Description:* Your public-facing e-commerce website has been defaced. No evidence of deeper system compromise initially, but reputation is at risk.
        *   *Factors to consider:* Public visibility, speed of restoration, evidence preservation, identifying root cause.
        *   *Proposed Containment:* ...
        *   *Justification:* ...

    *   **Scenario D: Insider Threat (Unauthorized Access)**
        *   *Description:* An internal employee with privileged access is observed accessing highly confidential customer records outside of their normal duties.
        *   *Factors to consider:* Legal implications, covert investigation, minimizing disruption, preserving employment relationship (potentially).
        *   *Proposed Containment:* ...
        *   *Justification:* ...

## Expected Output

```markdown
## Evaluation and Application of Containment Strategies

### Key Factors Influencing Containment Strategy
*   **Impact Assessment:** What is the potential or current damage to systems, data, and business operations?
*   **Evidence Preservation:** How critical is it to collect and preserve forensic evidence for legal or post-mortem analysis?
*   **Service Availability:** What is the business criticality of the affected systems and the acceptable downtime?
*   **Resources:** What personnel, tools, and budget are available for containment?
*   **Time:** How quickly is the incident spreading or causing damage?
*   **Detection & Analysis Maturity:** How well understood is the incident, its scope, and its attack vector?

### Incident Scenarios and Proposed Containment

1.  **Scenario A: Malware Outbreak (Worm/Ransomware)**
    *   **Proposed Containment:** Immediate network segmentation (e.g., disconnecting affected segments from the core network, isolating infected hosts). Block malicious indicators (IPs, domains) at firewalls/proxies. Disable external access to critical services.
    *   **Justification:** The primary goal is to halt the rapid spread and prevent further data destruction. Network segmentation is a crucial short-term measure to create a firebreak. Fast-moving threats require aggressive containment.

2.  **Scenario B: Data Exfiltration (Slow & Stealthy)**
    *   **Proposed Containment:** Covert monitoring of the compromised system and network traffic to fully understand the exfiltration method, scope, and destination. Once understood, implement targeted egress filtering (e.g., block destination IP/domain at firewall) to stop exfiltration without tipping off the attacker. Potentially disable specific network protocols if only used by the attacker.
    *   **Justification:** A slow and stealthy exfiltration requires careful analysis to avoid alerting the attacker, which could lead to accelerated exfiltration or destruction of evidence. Covert monitoring helps gather intelligence before a decisive containment action.

3.  **Scenario C: Web Server Compromise (Defacement)**
    *   **Proposed Containment:** Immediately take the defaced web server offline and replace it with a clean, pre-hardened instance or a "placeholder" page. Block the known attack vector (if identified). Preserve the compromised server for forensic analysis (image it).
    *   **Justification:** Public-facing defacement has immediate reputational impact. Rapid restoration with a clean version minimizes public exposure, while the original server is preserved for forensic investigation to determine the root cause.

4.  **Scenario D: Insider Threat (Unauthorized Access)**
    *   **Proposed Containment:** Immediately disable the employee's access to all sensitive systems and data. Initiate covert monitoring of their remaining activities (e.g., email, network traffic) if legal and policy allow, to gather further evidence. Secure their workstation. Engage legal and HR.
    *   **Justification:** Requires a sensitive approach due to legal/HR implications. Immediate access revocation limits further damage, while covert monitoring can provide valuable evidence for internal investigation and potential legal action. Evidence preservation is paramount.
```


## Reflection Questions

1.  Why is "containment" often a balancing act between stopping the attack and preserving business operations?
2.  How does the "severity" and "type" of an incident directly influence the choice of containment strategy?
3.  What are the risks of premature containment (e.g., cutting off network access too early without proper analysis)?

## Next Steps

*   [Outline Eradication and Recovery Steps](07-eradication-recovery-steps.md)
*   Further reading: [NIST SP 800-61 R2, Section 3.3](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

## Hints / Troubleshooting

*   **No Single Answer**: There's often no single "correct" containment strategy; the best approach depends on many variables.
*   **Think in Layers**: Containment often involves multiple layers (host, network, application).
*   **Documentation**: Always document your containment actions and their rationale.
