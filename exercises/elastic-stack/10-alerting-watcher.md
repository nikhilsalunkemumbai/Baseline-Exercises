---
Title: Set Up Alerting with Watcher
Objective: Configure a basic alert in Kibana using Watcher (or the newer Alerting feature) to detect specific patterns in your logs and trigger an action.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana, sample log data
Setup Instructions:
    - Complete Exercise 02 (Filebeat ingesting logs into Elasticsearch).
    - Ensure you have some log data with a pattern you want to alert on (e.g., `ERROR` messages, failed login attempts).
    - Note: Watcher is a paid feature in Elastic Stack. If you are using a basic/free license, you might need to use the newer Kibana Alerting and Actions framework which has some free tiers. This exercise will focus on the principles applicable to both.
---

## Steps

1.  **Access Kibana Alerting & Actions (or Watcher)**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Stack Management" -> "Alerts and Actions" -> "Rules" (for newer Kibana versions) or "Stack Management" -> "Watcher" (for older versions/if using paid features).

2.  **Create a New Rule (Alert)**
    *   Click "Create rule".
    *   **Name**: Give your rule a descriptive name, e.g., "High Error Rate Alert".
    *   **Tags**: Add optional tags, e.g., `security`, `critical`.
    *   **Space**: Select the appropriate space.

3.  **Define the Rule Condition**
    *   **Connector**: Select "Elasticsearch query" or "Index threshold" as the rule type.
    *   **Indices**: Specify the index pattern to monitor, e.g., `filebeat-*`.
    *   **Time field**: Use `@timestamp`.
    *   **Interval**: How often should the rule check for conditions (e.g., "every 1 minute").
    *   **Look back time**: How far back in time should each check look (e.g., "for the last 5 minutes").
    *   **Query**: Enter a KQL query to filter events, e.g., `log.level: ERROR`.
    *   **Conditions**:
        *   **`count()` of `documents`**:
        *   **`is above`**:
        *   **`1`**: (This means, if more than 1 error message in the last 5 minutes). Adjust threshold as needed.

4.  **Define the Action**
    *   **Action Type**: Click "Add action" and select an action type. For this exercise, we'll use a simple "Log" action if available, or "Webhook" if you have a simple endpoint to test against, or simply observe the rule status.
    *   **Name**: "Log to Console"
    *   **Message**: `Detected {{context.hits}} error events in the last 5 minutes. First error message: {{context.hits.0.message}}`
    *   (If using Webhook): Provide a URL and body.

5.  **Activate and Test the Rule**
    *   Click "Continue" or "Create rule".
    *   Ensure the rule is enabled.
    *   To test, add more `ERROR` messages to your `sample.log` file, or whatever log file Filebeat is monitoring.
        ```
        [2023-10-27 10:30:01] ERROR Another critical error occurred!
        [2023-10-27 10:30:02] ERROR Database connection failed.
        ```
    *   Wait for the specified interval.
    *   Check the "Rules" page for the status of your alert. It should transition to "Alerting" or show a triggered status if the condition is met.
    *   If you configured a "Log" action, check Kibana's internal logs (or the configured log output).

## Expected Output

```
# Screenshot of Kibana Alerting & Actions interface showing the created rule.
# Screenshot of the rule status changing to "Alerting" or "Triggered" after the condition is met.
```


## Reflection Questions

1.  How can alerting help in proactive security operations?
2.  What are the key components of a Kibana alert rule (condition, action, interval)?
3.  Besides logging, what other types of actions could be useful for security alerts?

## Next Steps

*   [Monitor Elasticsearch Cluster Health with Stack Monitoring](11-monitor-es-cluster-stack.md)
*   Further reading: [Kibana Alerting and Actions](https://www.elastic.co/guide/en/kibana/current/alerting-getting-started.html)
*   Further reading: [Watcher documentation (if applicable)](https://www.elastic.co/guide/en/elasticsearch/reference/current/xpack-alerting.html)

## Hints / Troubleshooting

*   **Rule not triggering**:
    *   Double-check your query in the rule definition. Test it in Discover first to ensure it returns the expected results.
    *   Ensure the threshold is correctly set.
    *   Verify the interval and look-back time are appropriate for your data flow.
    *   Make sure there is actual data matching your condition being ingested.
*   **Action not firing**:
    *   Check the Kibana server logs for any errors related to executing the action.
    *   If using Webhook, verify the endpoint is reachable and configured correctly.
*   **License**: Remember that advanced alerting features might require a paid Elastic Stack license.
