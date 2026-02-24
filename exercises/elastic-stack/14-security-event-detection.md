---
Title: Detect Security Events with Kibana Security
Objective: Utilize Kibana's Security app to detect and investigate common security events, analyze alerts, and understand security posture.
Estimated Time: 1.5 hours
Tools Needed: Elasticsearch, Kibana, sample security event data (e.g., from Auditbeat, Syslog, or custom security logs)
Setup Instructions:
    - Complete Exercise 01 (Elasticsearch and Kibana running).
    - Ingest some sample security event data into Elasticsearch. This can be done via Auditbeat or by generating custom logs. For this exercise, we'll assume you have some logs with fields like `event.category: authentication` and `event.outcome: failure`.
---

## Steps

1.  **Ingest Sample Security Event Data**
    *   **Option A: Using Auditbeat (Recommended for real data)**
        *   Install and configure Auditbeat to collect security events (e.g., Windows Event Logs, Linux Auditd) and send them to Elasticsearch. Follow Auditbeat's official documentation for setup.
    *   **Option B: Generating Custom Logs (for simulation)**
        *   Modify your `sample.log` (from Exercise 02) or create a new `security_events.log` with entries like:
            ```
            [2023-10-27 11:00:01] AUTHENTICATION_FAILURE user=baduser ip=192.168.1.10 login_attempt=5
            [2023-10-27 11:00:05] AUTHENTICATION_SUCCESS user=admin ip=10.0.0.5
            [2023-10-27 11:00:10] PROCESS_CREATION event_id=4688 process_name=evil.exe parent_process=explorer.exe
            [2023-10-27 11:00:15] AUTHENTICATION_FAILURE user=attacker ip=203.0.113.1 login_attempt=1
            ```
        *   Configure Filebeat to ingest `security_events.log` into an index like `security_events-*`. You'll need an ingest pipeline with `grok` patterns to parse these fields into `event.category`, `event.outcome`, `user.name`, `source.ip`, etc.

2.  **Access Kibana Security App**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Security" -> "Overview".
    *   This dashboard provides a high-level view of security alerts, hosts, network activity, and recent events.

3.  **Explore Alerts and Detections**
    *   Navigate to "Security" -> "Detections" -> "Rules".
    *   Elastic Security comes with pre-built detection rules. Enable a few relevant rules (e.g., "Multiple Failed Login Attempts" if you have authentication logs).
    *   Navigate to "Security" -> "Detections" -> "Alerts".
    *   If your ingested data matches the criteria for any enabled rules, you should start seeing alerts generated here.
    *   Click on an alert to view its details, including the raw event, related events, and MITRE ATT&CK tactics/techniques.

4.  **Investigate Hosts and Network Events**
    *   Navigate to "Security" -> "Hosts". This view provides insights into host-centric events (e.g., process activity, network connections, user logins).
    *   Navigate to "Security" -> "Network". This view focuses on network traffic, DNS queries, and flow data.

5.  **Create a Custom Detection Rule (Optional)**
    *   Go to "Security" -> "Detections" -> "Rules" and click "Create new rule".
    *   Select "Custom query".
    *   **Name**: "Custom Bruteforce Attempt"
    *   **Query**: `event.category: authentication and event.outcome: failure and source.ip: "203.0.113.1"` (or an IP from your sample data).
    *   **Interval**: "Every 1 minute".
    *   **Severity**: "High".
    *   **Actions**: Configure an action similar to Exercise 10 (e.g., log to console, send an email).
    *   Activate the rule and observe if it generates an alert when matching data is ingested.

## Expected Output

```
# Screenshot of the Kibana Security "Overview" dashboard.
# Screenshot of the "Alerts" page showing a triggered alert (e.g., "Multiple Failed Login Attempts").
# Screenshot of an alert details page, showing event context.
```


## Reflection Questions

1.  How does the Kibana Security app consolidate various security data sources for threat detection and investigation?
2.  What is the value of correlating security events with MITRE ATT&CK framework data?
3.  How can custom detection rules help tailor your security monitoring to specific threats in your environment?

## Next Steps

*   [Basic Elasticsearch Query DSL](15-es-query-dsl-basics.md)
*   Further reading: [Elastic Security Overview](https://www.elastic.co/guide/en/security/current/overview.html)
*   Further reading: [Auditbeat Quick Start](https://www.elastic.co/guide/en/beats/auditbeat/current/auditbeat-getting-started.html)

## Hints / Troubleshooting

*   **No security data**: Ensure Auditbeat (or your custom Filebeat configuration) is correctly configured and sending data to the correct Elasticsearch index.
*   **No alerts**:
    *   Verify that you have enabled relevant detection rules.
    *   Check that your ingested data actually matches the criteria defined in the rules. Use Discover to test queries.
    *   Ensure the time range in the Security app encompasses your data.
*   **Permissions**: Kibana Security app features might be restricted based on your user's roles and permissions.
