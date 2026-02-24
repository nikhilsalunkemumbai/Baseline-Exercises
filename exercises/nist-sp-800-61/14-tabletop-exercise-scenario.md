---
Title: Outline a Tabletop Exercise Scenario
Objective: Design a structured tabletop exercise scenario for a simulated security incident to test an organization's incident response plan, team coordination, and decision-making processes, as part of the NIST Preparation phase.
Estimated Time: 1.5 hours
Tools Needed: Text editor or presentation software (conceptual outline), NIST SP 800-61 R2 document
Setup Instructions:
    - Review Section 3.1 "Preparation" in NIST SP 800-61 R2, specifically the importance of testing incident response capabilities.
    - Recall elements from previous exercises: incident categorization, communication plans, eradication/recovery steps.
---

## Steps

1.  **Understand the Purpose of Tabletop Exercises**
    *   Briefly explain why tabletop exercises are a valuable tool for incident response readiness, especially in the "Preparation" phase.
    *   *Hint:* Focus on testing plans, identifying gaps, improving communication, and team cohesion.

2.  **Define Exercise Objectives**
    *   For your simulated scenario, what do you want to achieve with the exercise? (e.g., "Test communication plan," "Assess decision-making under pressure," "Identify gaps in the IR plan"). List 3-5 clear objectives.

3.  **Choose an Incident Scenario**
    *   Select a realistic and relevant incident type for a tabletop exercise. Consider something impactful but manageable for a simulation.
    *   *Example:* "Major Ransomware Attack with Data Exfiltration" (building on previous exercises).
    *   Provide a brief **Scenario Overview** to set the stage.

4.  **Develop Initial Inject and Subsequent Injects**
    *   **Initial Inject:** This is the first piece of information that kicks off the exercise. It should be presented to participants without warning and simulate how a real incident might begin.
        *   *Example:* "It's 9:00 AM on a Monday. The help desk reports multiple users are locked out of their systems, and files on shared drives are inaccessible and renamed with a `.crypt` extension. An internal security alert flags suspicious outbound connections from the finance department's servers."
    *   **Subsequent Injects:** These are pieces of information, new developments, or challenges introduced throughout the exercise to keep it dynamic and test specific aspects of the IR plan.
        *   Plan at least 3-5 subsequent injects, each designed to elicit specific responses from the participants.
        *   *Examples:* "A prominent news outlet contacts PR asking about rumors of a data breach," "Legal counsel advises on GDPR notification requirements," "A critical database server is reported to be offline."

5.  **Identify Expected Outcomes / Discussion Points**
    *   For each inject (especially the initial one), consider what specific actions, decisions, or discussions you would expect from the participants. These will serve as evaluation points during the exercise.
    *   *Examples:* "IR team mobilizes, CISO informed," "Legal/PR consulted," "Containment strategies discussed."

6.  **Define Participants and Roles**
    *   List the key roles/departments that should participate in this exercise (e.g., CISO, IR Lead, Network Admin, Legal, HR, PR).

## Expected Output

```markdown
## Tabletop Exercise Scenario Outline: Major Ransomware Attack

### 1. Purpose of Tabletop Exercises
Tabletop exercises are discussion-based sessions where participants talk through their roles and responsibilities during an incident. They are crucial for:
*   Testing the Incident Response Plan (IRP) in a low-stress environment.
*   Identifying gaps, ambiguities, and weaknesses in existing policies, procedures, and communication plans.
*   Improving coordination and communication among IR team members and other stakeholders.
*   Enhancing team members' understanding of their roles and decision-making under simulated pressure.
*   Fostering cross-departmental collaboration (IT, Legal, HR, PR, Management).

### 2. Exercise Objectives
This tabletop exercise aims to:
*   Test the effectiveness of the organization's current Incident Response Plan (IRP) against a multi-stage ransomware attack.
*   Evaluate the incident response team's ability to detect, analyze, contain, eradicate, and recover from a widespread malware infection.
*   Assess internal and external communication strategies with various stakeholders (e.g., management, legal, PR, potentially customers).
*   Identify critical decision points and escalation paths under pressure.
*   Determine the readiness for legal and regulatory compliance obligations (e.g., data breach notification).

### 3. Scenario Overview
It's a typical Tuesday morning. Employees are starting their workday. Suddenly, reports begin flooding the IT Help Desk: multiple users are locked out of their computers, and files on shared network drives are inaccessible and have a strange new extension (e.g., `.locked` or `.encrypt`). Shortly after, an automated alert from the EDR (Endpoint Detection and Response) system fires, indicating suspicious file encryption activity and unusual outbound network traffic from several servers, including those in the Finance department. A ransom note appears on affected workstations.

### 4. Participants and Roles
*   **Incident Response Lead:** Facilitates the IR process, coordinates the team.
*   **CISO (Chief Information Security Officer):** Strategic oversight, executive communication.
*   **Security Analyst(s):** Technical investigation, analysis, tool operation.
*   **Network Administrator(s):** Network segmentation, traffic monitoring.
*   **System Administrator(s):** Server management, system restoration.
*   **Legal Counsel:** Advises on legal obligations, evidence preservation, regulatory reporting.
*   **Public Relations / Communications:** Manages external messaging.
*   **HR Representative:** Advises on employee-related aspects.
*   **Business Unit Representative (e.g., Finance Manager):** Provides business impact perspective, prioritization.

### 5. Exercise Injects and Expected Outcomes

#### Initial Inject: (Presented without warning)
*   "It's 9:00 AM. The help desk reports a sudden surge in calls about users being locked out of their systems. Files on shared drives are inaccessible, renamed with a `.locked` extension, and a ransom note is visible on affected workstations. EDR alerts are firing for suspicious encryption activity and unusual outbound connections from Finance servers."
*   **Expected Outcomes:**
    *   IR Team mobilizes, Incident Lead notified.
    *   Initial incident declared, severity/priority assigned (HIGH).
    *   CISO and relevant management informed.
    *   Initial assessment of scope and impact initiated.
    *   Discussion of immediate containment actions (e.g., network segmentation, isolating affected hosts).
    *   Legal and PR are put on standby.

#### Inject #1: (30 minutes into exercise)
*   "Forensic analysis of a compromised workstation reveals the initial infection vector was a highly sophisticated phishing email with a malicious attachment, sent to several finance department employees. One employee clicked the link/opened the attachment yesterday afternoon."
*   **Expected Outcomes:**
    *   Discussion of eradication strategy (e.g., email gateway review, user awareness training).
    *   Confirmation of initial access vector.
    *   Expansion of forensic scope to email systems.
    *   Consideration of data exfiltration risk due to prolonged dwell time.

#### Inject #2: (60 minutes into exercise)
*   "A prominent news outlet contacts your PR department asking about rumors of a 'massive data breach' at your company, citing anonymous sources and hinting at potential customer data compromise. Social media chatter is also picking up."
*   **Expected Outcomes:**
    *   PR and Legal teams fully engage.
    *   Discussion of external communication strategy and public statement formulation.
    *   Legal counsel advises on potential breach notification requirements (e.g., GDPR, CCPA).
    *   Emphasis on factual, non-speculative communication.

#### Inject #3: (90 minutes into exercise)
*   "While containing the spread, the security team discovers that before files were encrypted, several gigabytes of data from a critical customer database on a separate server were exfiltrated to an unknown external IP address. The database contains PII for approximately 50,000 customers."
*   **Expected Outcomes:**
    *   Revised impact assessment (data confidentiality compromised).
    *   Immediate consultation with Legal on breach notification requirements.
    *   Refinement of containment to stop further exfiltration.
    *   Discussion on contacting affected customers, public relations implications.
    *   Prioritization of data recovery for the exfiltrated database.

#### Inject #4: (120 minutes into exercise)
*   "After several hours, the IR team successfully contains the ransomware, but not before 50% of the company's servers and 70% of employee workstations are encrypted. Recovery efforts are estimated to take 3-5 days. Senior management asks for an update on expected business impact and potential fines/liabilities."
*   **Expected Outcomes:**
    *   Review of eradication and recovery plan.
    *   Detailed business impact assessment.
    *   Legal provides preliminary assessment of fines/liabilities.
    *   Discussion on employee support during downtime.
    *   Planning for post-incident review and lessons learned.
```
![Expected output](images/exercise-14-output.png)

## Reflection Questions

1.  How does a tabletop exercise help identify gaps in an incident response plan that might not be apparent on paper?
2.  What are the benefits of including participants from non-technical departments (e.g., Legal, PR, HR) in a tabletop exercise?
3.  How can the injects be designed to maximize learning and challenge the participants effectively?

## Next Steps

*   [Define Key Metrics for Measuring Incident Response Effectiveness](15-metrics-reporting.md)
*   Further reading: [NIST SP 800-61 R2, Section 3.1.5 (Testing the Plan)](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

## Hints / Troubleshooting

*   **Realism**: Make the scenario as realistic as possible to your organization's context.
*   **Facilitation**: A good facilitator is key to guiding discussion and introducing injects effectively.
*   **No Right/Wrong Answers**: Emphasize learning and discussion over finding the "perfect" solution during the exercise.
*   **Debriefing**: The post-exercise debriefing is the most important part; this is where lessons are truly learned.
