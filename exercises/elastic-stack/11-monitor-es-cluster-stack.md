---
Title: Monitor Elasticsearch Cluster Health with Stack Monitoring
Objective: Utilize Kibana's built-in Stack Monitoring feature to gain insights into the health, performance, and resource utilization of your Elasticsearch cluster and Kibana instance.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana, at least one running Elasticsearch node and Kibana instance
Setup Instructions:
    - Complete Exercise 01 (Elasticsearch and Kibana running).
    - Have some data flowing into Elasticsearch (e.g., from Exercise 02 or 07).
---

## Steps

1.  **Access Kibana Stack Monitoring**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Observability" -> "Stack Monitoring".

2.  **Overview of Cluster Health**
    *   The "Overview" page provides a high-level summary of your entire Elastic Stack deployment.
    *   Observe the cluster status (green, yellow, red), node count, shard count, and overall indexing and search rates.
    *   Identify any warning or error messages that might indicate problems.

3.  **Explore Elasticsearch Nodes**
    *   Click on "Nodes" under "Elasticsearch" in the left navigation.
    *   You will see a list of all your Elasticsearch nodes. If you have a single-node cluster, you'll see just one.
    *   Click on your node name to drill down into its specific metrics, including CPU usage, JVM memory, disk usage, and network activity.
    *   Analyze the graphs to understand the node's performance over time.

4.  **Examine Elasticsearch Indices**
    *   Click on "Indices" under "Elasticsearch" in the left navigation.
    *   This view shows all indices in your cluster, their size, document count, and shard distribution.
    *   Identify which indices are consuming the most disk space or receiving the most writes/reads.

5.  **Monitor Kibana Instance**
    *   Click on "Instances" under "Kibana" in the left navigation.
    *   This page displays metrics for your Kibana instance, such as memory usage, connections, and HTTP response times.
    *   Identify potential performance bottlenecks in Kibana.

6.  **Analyze Alerts and History**
    *   Stack Monitoring also provides an "Alerts" section where you can see any alerts triggered by monitoring thresholds.
    *   The "History" tab allows you to look at past performance data.

## Expected Output

```
# Screenshot of the Kibana Stack Monitoring "Overview" page showing the cluster status and key metrics.
# Screenshot of an Elasticsearch node detail page showing resource utilization graphs (CPU, Memory).
```


## Reflection Questions

1.  Why is it important to monitor the health of your Elasticsearch cluster?
2.  What specific metrics would you pay close attention to during an incident, and why?
3.  How can Stack Monitoring help in capacity planning for your Elastic Stack deployment?

## Next Steps

*   [Ingest Nginx Web Logs](12-ingest-web-logs-nginx.md)
*   Further reading: [Monitor an Elastic Stack deployment](https://www.elastic.co/guide/en/kibana/current/xpack-monitoring.html)

## Hints / Troubleshooting

*   **No data in Stack Monitoring**:
    *   Ensure that monitoring is enabled in your `elasticsearch.yml` and `kibana.yml` configuration files. By default, it's often enabled or can be activated via x-pack settings.
    *   Verify that your Elasticsearch and Kibana instances are part of the same monitoring cluster.
    *   Check Kibana and Elasticsearch logs for any errors related to monitoring data collection.
*   **Performance overhead**: Monitoring itself consumes some resources. In very large production environments, you might dedicate separate monitoring clusters. For a single-node setup, the overhead is minimal.
