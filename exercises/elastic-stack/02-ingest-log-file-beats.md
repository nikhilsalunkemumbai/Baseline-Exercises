---
Title: Ingest a Log File with Filebeat
Objective: Configure Filebeat to read a sample log file and send its contents to Elasticsearch.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana, Filebeat, a sample log file
Setup Instructions:
    - Complete Exercise 01 (Elasticsearch and Kibana running).
    - Download and extract Filebeat (matching Elasticsearch version).
    - Create a sample log file, e.g., `sample.log` with a few lines of text.
---

## Steps

1.  **Download and Configure Filebeat**
    *   Download Filebeat from the official Elastic website.
    *   Extract the archive to a directory (e.g., `C:\elastic\filebeat`).
    *   Edit the `filebeat.yml` configuration file:
        *   In the `filebeat.inputs` section, uncomment and configure a `log` input:
            ```yaml
            - type: log
              enabled: true
              paths:
                - C:\path	o\your\sample.log # Change this to your log file path
            ```
        *   In the `output.elasticsearch` section, ensure `hosts` points to your Elasticsearch instance (usually `["localhost:9200"]`).
        *   Ensure `cloud.id` and `cloud.auth` are commented out unless you are using Elastic Cloud.
        *   Uncomment and configure `setup.kibana.host` to `localhost:5601`.
            ```yaml
            output.elasticsearch:
              hosts: ["localhost:9200"]
              username: "elastic" # If security is enabled
              password: "YOUR_ELASTIC_PASSWORD" # If security is enabled

            setup.kibana:
              host: "localhost:5601"
              username: "elastic" # If security is enabled
              password: "YOUR_ELASTIC_PASSWORD" # If security is enabled
            ```
            **Note**: Replace `YOUR_ELASTIC_PASSWORD` with the password obtained during Elasticsearch setup.

2.  **Create a Sample Log File**
    *   Create a file named `sample.log` (or any name you configured) in the path specified in `filebeat.yml`.
    *   Add some sample log entries, e.g.:
        ```
        [2023-10-27 10:00:01] INFO User 'admin' logged in from 192.168.1.100
        [2023-10-27 10:00:05] WARNING Failed login attempt for 'guest' from 10.0.0.5
        [2023-10-27 10:00:10] ERROR Disk full on /dev/sda1
        ```

3.  **Setup Filebeat Dashboards and Start Filebeat**
    *   Open a terminal/command prompt in the Filebeat directory (`C:\elastic\filebeat`) and run:
        ```bash
        filebeat.exe setup -e # -e for verbose output
        ```
        This command loads the default Filebeat index template and Kibana dashboards.
    *   Once setup is complete, start Filebeat:
        ```bash
        filebeat.exe -e
        ```
    *   Observe the output for any errors and confirm logs are being sent.

4.  **Verify Data in Kibana Discover**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Analytics" -> "Discover".
    *   If prompted, create an index pattern for `filebeat-*`.
    *   You should see your log entries from `sample.log` listed in Discover. You might need to adjust the time range filter (e.g., to "Last 15 minutes" or "Last 1 hour").

## Expected Output

```
# Example Filebeat console output showing events published
2023-10-27T10:05:00.123+0530	INFO	[publisher_pipeline_output]	pipeline/output.go:154	Connecting to backoff(elasticsearch(http://localhost:9200))
2023-10-27T10:05:00.123+0530	INFO	[publisher_pipeline_output]	pipeline/output.go:162	Connection to backoff(elasticsearch(http://localhost:9200)) established
2023-10-27T10:05:00.456+0530	INFO	[input]	log/input.go:157	Started tracking changes for a new file: C:\path	o\your\sample.log
2023-10-27T10:05:00.789+0530	INFO	[monitoring]	log/log.go:144	Non-zero metrics in the last 30s: filebeat.events.active=0 filebeat.events.added=3 filebeat.events.done=3 filebeat.input.bytes_read=177 ...

# Screenshot of Kibana Discover showing log entries
```


## Reflection Questions

1.  What is the role of Filebeat in the Elastic Stack?
2.  How does Filebeat know which files to monitor and where to send the data?
3.  What are some common issues when configuring Filebeat and how would you troubleshoot them?

## Next Steps

*   [Search Data in Kibana Discover](03-search-data-kibana.md)
*   Further reading: [Filebeat Quick Start](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-getting-started.html)

## Hints / Troubleshooting

*   **Filebeat not starting**: Check `filebeat.yml` for syntax errors. Use `filebeat test config` and `filebeat test output` to validate your configuration.
*   **Logs not appearing in Kibana**:
    *   Ensure Elasticsearch and Kibana are running.
    *   Check Filebeat logs for errors (run with `-e` flag).
    *   Verify the `filebeat-*` index pattern exists in Kibana (Management -> Index Patterns).
    *   Adjust the time range in Kibana Discover.
    *   Ensure the `username` and `password` for Elasticsearch and Kibana are correctly configured in `filebeat.yml` if security is enabled.
