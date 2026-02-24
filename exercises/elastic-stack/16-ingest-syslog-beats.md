---
Title: Ingest Syslog Data with Filebeat
Objective: Configure Filebeat to collect syslog messages from a Linux system and send them to Elasticsearch for centralized logging and analysis.
Estimated Time: 1-1.5 hours
Tools Needed: Elasticsearch, Kibana, Filebeat, a Linux machine (physical or VM) generating syslog data
Setup Instructions:
    - Complete Exercise 01 (Elasticsearch and Kibana running).
    - Complete Exercise 02 (Filebeat installed and basic config understood).
    - Have access to a Linux machine (e.g., Ubuntu, CentOS) where you can install and configure Filebeat.
---

## Steps

1.  **Install Filebeat on Linux Machine**
    *   SSH into your Linux machine.
    *   Follow the official Filebeat installation guide for your Linux distribution.
        *   **Debian/Ubuntu:**
            ```bash
            sudo apt-get update && sudo apt-get install filebeat
            ```
        *   **RHEL/CentOS:**
            ```bash
            sudo yum install filebeat
            ```
        *   Alternatively, download the `.tar.gz` and extract it.

2.  **Configure Filebeat to Collect Syslog**
    *   Edit the `filebeat.yml` configuration file, typically located at `/etc/filebeat/filebeat.yml`.
    *   **Enable the `system` module**: This module includes configurations for syslog.
        ```yaml
        filebeat.config.modules:
          path: ${path.config}/modules.d/*.yml
          reload.enabled: false
        ```
        Then, enable the `system` module:
        ```bash
        sudo filebeat modules enable system
        ```
    *   **Configure Elasticsearch Output**: Ensure the output section points to your Elasticsearch instance (e.g., `localhost:9200` if on the same host, or `your_elasticsearch_ip:9200`). Replace `YOUR_ELASTIC_PASSWORD` with your actual password.
        ```yaml
        output.elasticsearch:
          hosts: ["<your_elasticsearch_ip>:9200"] # e.g., "192.168.1.50:9200"
          username: "elastic"
          password: "YOUR_ELASTIC_PASSWORD"
          ssl.enabled: true
          ssl.verification_mode: none # For local testing; use certificate verification in production
        ```
    *   **Configure Kibana Setup**:
        ```yaml
        setup.kibana:
          host: "<your_kibana_ip>:5601" # e.g., "192.168.1.50:5601"
          username: "elastic"
          password: "YOUR_ELASTIC_PASSWORD"
          ssl.enabled: true
          ssl.verification_mode: none
        ```

3.  **Setup Filebeat Assets and Start Service**
    *   Run the setup command to load dashboards and index templates:
        ```bash
        sudo filebeat setup
        ```
    *   Start the Filebeat service:
        ```bash
        sudo service filebeat start
        # Or for systems using systemd:
        # sudo systemctl start filebeat
        # sudo systemctl enable filebeat
        ```
    *   Check the Filebeat logs for any errors:
        ```bash
        sudo journalctl -u filebeat -f
        # Or check /var/log/filebeat/filebeat (if configured)
        ```

4.  **Generate Some Syslog Events**
    *   On your Linux machine, you can generate some test syslog messages:
        ```bash
        logger "This is a test syslog message from the THIR exercise."
        logger -p local0.warn "WARNING: This is a warning message."
        ```
    *   Perform some actions that generate system logs (e.g., failed `sudo` attempts, installing packages).

5.  **Verify Data in Kibana**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Analytics" -> "Discover".
    *   Select the `filebeat-*` index pattern.
    *   You should see your Linux syslog messages, parsed into structured fields (e.g., `system.syslog.message`, `log.level`, `host.name`).
    *   (Optional) Go to "Dashboard" and search for the "Filebeat System Overview" dashboard (loaded by `filebeat setup`). This dashboard will visualize your system and syslog data.

## Expected Output

```
# Output from `sudo filebeat setup` indicating success.
# Filebeat logs showing events being published.
# Screenshot of Kibana Discover showing parsed syslog messages from your Linux machine.
# Optional: Screenshot of "Filebeat System Overview" dashboard in Kibana.
```


## Reflection Questions

1.  How does Filebeat's `system` module simplify the ingestion of common system logs like syslog?
2.  What benefits does centralized syslog collection offer for incident response and security monitoring?
3.  Compare and contrast collecting logs directly from Filebeat vs. forwarding them to Logstash first. When would you choose one over the other?

## Next Steps

*   Proceed to the next set of exercises for NIST SP 800-61.
*   Further reading: [Filebeat System Module](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-module-system.html)

## Hints / Troubleshooting

*   **Connectivity issues**: Ensure your Linux machine can reach your Elasticsearch and Kibana instances (firewall rules, network connectivity).
*   **Permissions**: Filebeat needs read permissions for log files (e.g., `/var/log/syslog`, `/var/log/auth.log`).
*   **No data in Kibana**:
    *   Check Filebeat logs (`sudo journalctl -u filebeat -f`).
    *   Verify Elasticsearch and Kibana are running.
    *   Ensure the `filebeat-*` index pattern is selected in Discover.
    *   Adjust the time range in Kibana.
*   **Time synchronization**: Ensure the clock on your Linux machine is synchronized (e.g., with NTP) to avoid timestamp discrepancies in Kibana.
