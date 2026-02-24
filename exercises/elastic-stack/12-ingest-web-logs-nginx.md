---
Title: Ingest Nginx Web Logs with Filebeat
Objective: Configure Filebeat to ingest Nginx access logs and use an Elasticsearch ingest pipeline to parse the log data into structured fields.
Estimated Time: 1.5 hours
Tools Needed: Elasticsearch, Kibana, Filebeat, Nginx (or sample Nginx logs)
Setup Instructions:
    - Complete Exercise 01 (Elasticsearch and Kibana running).
    - Complete Exercise 02 (Filebeat installed and basic config understood).
    - Have Nginx installed and generating access logs, or create a sample Nginx access log file.
---

## Steps

1.  **Generate Sample Nginx Logs (or use existing)**
    *   If you have Nginx running, ensure it's generating access logs (default location on Linux is `/var/log/nginx/access.log`, on Windows it might be in Nginx installation directory under `logs`).
    *   If not, create a `nginx_access.log` file with sample entries like:
        ```
        192.168.1.1 - - [27/Oct/2023:10:05:01 +0000] "GET /index.html HTTP/1.1" 200 157 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36"
        10.0.0.5 - - [27/Oct/2023:10:05:10 +0000] "POST /api/login HTTP/1.1" 401 234 "http://example.com/login" "curl/7.81.0"
        172.16.0.10 - - [27/Oct/2023:10:05:20 +0000] "GET /admin/dashboard HTTP/1.1" 302 0 "http://example.com/admin" "Edge/117.0.2045.60"
        ```

2.  **Create an Elasticsearch Ingest Pipeline**
    *   This pipeline will parse the Nginx common log format into structured fields.
    *   In Kibana Dev Tools (`http://localhost:5601/app/dev_tools`), execute the following PUT request:
        ```json
        PUT _ingest/pipeline/nginx_access_pipeline
        {
          "description": "Parses Nginx common access log format",
          "processors": [
            {
              "grok": {
                "field": "message",
                "patterns": [
                  "%{IPORHOST:nginx.access.remote_ip} - %{DATA:nginx.access.user_name} \[%{HTTPDATE:nginx.access.time}\] "%{WORD:nginx.access.method} %{DATA:nginx.access.url} HTTP/%{NUMBER:nginx.access.http_version}" %{NUMBER:nginx.access.response_code} %{NUMBER:nginx.access.body_sent.bytes} "%{DATA:nginx.access.referrer}" "%{DATA:nginx.access.agent}""
                ],
                "ignore_missing": true
              }
            },
            {
              "date": {
                "field": "nginx.access.time",
                "formats": ["dd/MMM/yyyy:HH:mm:ss Z"],
                "target_field": "@timestamp"
              }
            },
            {
              "remove": {
                "field": "nginx.access.time"
              }
            },
            {
              "convert": {
                "field": "nginx.access.response_code",
                "type": "integer"
              }
            },
            {
              "convert": {
                "field": "nginx.access.body_sent.bytes",
                "type": "long"
              }
            },
            {
              "geoip": {
                "field": "nginx.access.remote_ip",
                "target_field": "nginx.access.geoip",
                "ignore_failure": true
              }
            }
          ]
        }
        ```
    *   Confirm the pipeline is created successfully.

3.  **Configure Filebeat for Nginx Logs**
    *   Edit your `filebeat.yml` configuration file (e.g., `C:\elastic\filebeat\filebeat.yml`).
    *   Add a new `filebeat.inputs` section for Nginx logs, pointing it to your Nginx access log file and specifying the ingest pipeline:
        ```yaml
        - type: log
          enabled: true
          paths:
            - C:\path	o
ginx_access.log # Adjust to your Nginx access log path
          fields:
            log_type: nginx_access # Custom field to identify log type
          processors:
            - add_fields:
                target: ""
                fields:
                  pipeline: nginx_access_pipeline # Apply the ingest pipeline
        ```
    *   Make sure `output.elasticsearch` is configured correctly, similar to Exercise 02. You might want to send these logs to a separate index for better organization.
        ```yaml
        output.elasticsearch:
          hosts: ["localhost:9200"]
          index: "nginx-access-%{+YYYY.MM.dd}" # New index for Nginx logs
          user: "elastic"
          password: "YOUR_ELASTIC_PASSWORD"
          ssl_enabled: true
          ssl_certificate_verification: false
        ```

4.  **Start Filebeat**
    *   Open a terminal/command prompt in the Filebeat directory and run:
        ```bash
        filebeat.exe -e
        ```
    *   Observe Filebeat's output for any errors.

5.  **Verify Data in Kibana Discover**
    *   Open Kibana (`http://localhost:5601`).
    *   Navigate to "Stack Management" -> "Index Patterns" and create a new index pattern for `nginx-access-*`.
    *   Go to "Analytics" -> "Discover" and select the `nginx-access-*` index pattern.
    *   You should see your Nginx logs parsed into individual fields like `nginx.access.remote_ip`, `nginx.access.method`, `nginx.access.response_code`, and potentially `nginx.access.geoip`.

## Expected Output

```
# Output from Kibana Dev Tools confirming pipeline creation:
{
  "acknowledged": true
}

# Screenshot of Kibana Discover showing parsed Nginx log entries with structured fields (e.g., remote_ip, method, response_code).
```


## Reflection Questions

1.  What is the purpose of an Elasticsearch ingest pipeline, and how does it differ from Logstash for data processing?
2.  How does parsing raw log data into structured fields enhance your ability to analyze and visualize it?
3.  Why is it beneficial to have separate indices for different types of log data (e.g., `filebeat-*` vs. `nginx-access-*`)?

## Next Steps

*   [Use Data Views in Kibana](13-data-views-kibana.md)
*   Further reading: [Filebeat Nginx module (alternative for simpler setup)](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-module-nginx.html)
*   Further reading: [Elasticsearch Ingest Pipelines](https://www.elastic.co/guide/en/elasticsearch/reference/current/ingest-processors.html)

## Hints / Troubleshooting

*   **Pipeline parsing errors**:
    *   Use the `_simulate` API in Dev Tools to test your `grok` patterns:
        ```json
        POST _ingest/pipeline/nginx_access_pipeline/_simulate
        {
          "docs": [
            {
              "_source": {
                "message": "192.168.1.1 - - [27/Oct/2023:10:05:01 +0000] "GET /index.html HTTP/1.1" 200 157 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36""
              }
            }
          ]
        }
        ```
    *   Adjust `grok` patterns until parsing is successful.
*   **Logs not appearing**:
    *   Check Filebeat logs for errors.
    *   Verify the `pipeline` field is correctly specified in `filebeat.yml` under the processor.
    *   Ensure the index name in `output.elasticsearch` matches what you're creating the index pattern for.
*   **Time field not recognized**: Double-check the `date` processor in the ingest pipeline for correct format matching.
