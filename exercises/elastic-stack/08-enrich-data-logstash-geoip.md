---
Title: Enrich Data with Logstash GeoIP
Objective: Use the Logstash GeoIP filter to enrich log data with geographical information (country, city, coordinates) based on IP addresses, enabling location-based analysis in Kibana.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana, Logstash, sample log data with IP addresses
Setup Instructions:
    - Complete Exercise 02 (Filebeat ingesting logs) or have some log data with IP addresses (e.g., source IP, destination IP) in Elasticsearch.
    - Complete Exercise 07 (Logstash installed and basic config understood).
    - Ensure Logstash has internet access to download GeoIP databases or you have them locally.
---

## Steps

1.  **Prepare Sample Log Data with IP Addresses**
    *   If you're continuing from Exercise 02, modify your `sample.log` to include IP addresses, or create a new `ip_logs.log` file.
        ```
        [2023-10-27 10:00:01] INFO User 'admin' logged in from 192.168.1.100
        [2023-10-27 10:00:05] WARNING Failed login attempt for 'guest' from 203.0.113.45 (Australia)
        [2023-10-27 10:00:10] ERROR Connection denied from 8.8.8.8 (USA, Google DNS)
        [2023-10-27 10:00:15] INFO Successful connection from 185.199.108.153 (USA, GitHub)
        ```
    *   Update your Filebeat configuration (`filebeat.yml`) to ingest this new `ip_logs.log` or modify the existing one. Restart Filebeat to push this new data.

2.  **Create Logstash GeoIP Configuration**
    *   In the `C:\elastic\logstash\config` directory, create a new file named `geoip_enrichment.conf`.
    *   Add the following Logstash configuration:
        ```conf
        input {
          beats {
            port => 5044 # Filebeat sends data to this port by default
          }
        }
        filter {
          # Assume 'client_ip' or 'source_ip' field contains the IP address
          # You might need a grok filter before this if your IP is part of a message field
          grok {
            match => { "message" => "Connection denied from %{IP:source_ip}" }
            tag_on_failure => ["grok_failed"]
          }
          grok {
            match => { "message" => "logged in from %{IP:source_ip}" }
            tag_on_failure => ["grok_failed"]
          }
          grok {
            match => { "message" => "Failed login attempt for %{GREEDYDATA} from %{IP:source_ip}" }
            tag_on_failure => ["grok_failed"]
          }
          geoip {
            source => "source_ip"
            target => "geoip"
            # Optional: Specify database path if not using default download location
            # database => "/path/to/GeoLite2-City.mmdb"
          }
        }
        output {
          elasticsearch {
            hosts => ["localhost:9200"]
            index => "enriched_logs-%{+YYYY.MM.dd}"
            user => "elastic"
            password => "YOUR_ELASTIC_PASSWORD" # Replace with your elastic user password
            ssl_enabled => true
            ssl_certificate_verification => false # For local testing
          }
          stdout { codec => rubydebug } # Optional: for debugging
        }
        ```
        **Note**: Replace `YOUR_ELASTIC_PASSWORD` with your `elastic` user password. The `grok` filters are examples; adjust them to extract the IP address into a field named `source_ip` from your specific log messages.

3.  **Run Logstash with GeoIP Configuration**
    *   Open a terminal/command prompt in the Logstash directory (`C:\elastic\logstash`) and run:
        ```bash
        bin\logstash.bat -f config\geoip_enrichment.conf --config.reload.automatic # For Windows
        # bin/logstash -f config/geoip_enrichment.conf --config.reload.automatic # For Linux/macOS
        ```
    *   If you encounter issues with GeoIP database download, ensure Logstash has outbound internet access.
    *   Restart Filebeat so it sends new data to Logstash, which then processes it.

4.  **Verify Enriched Data in Kibana**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Management" -> "Stack Management" -> "Index Patterns".
    *   Create a new index pattern for `enriched_logs-*`.
    *   Go to "Analytics" -> "Discover" and select the `enriched_logs-*` index pattern.
    *   Expand a document that had an IP address. You should see a new `geoip` field containing sub-fields like `country_name`, `city_name`, `location` (with latitude/longitude), etc.
    *   (Optional) Go to "Analytics" -> "Maps" and try to visualize your logs on a map using the `geoip.location` field.

## Expected Output

```
# Logstash console output showing geoip enrichment
{
           "message" => "[2023-10-27 10:00:05] WARNING Failed login attempt for 'guest' from 203.0.113.45 (Australia)",
          "source_ip" => "203.0.113.45",
             "geoip" => {
                   "continent_code" => "OC",
                     "country_name" => "Australia",
                     "country_code2" => "AU",
                     "country_code3" => "AUS",
                            "ip" => "203.0.113.45",
                       "latitude" => -33.494,
                      "longitude" => 143.2104,
                         "location" => {
                        "lat" => -33.494,
                        "lon" => 143.2104
                    },
                         "region_code" => "NSW",
                         "region_name" => "New South Wales",
                          "city_name" => "Sydney",
                            "timezone" => "Australia/Sydney",
                             "postal_code" => "2000"
            }
        # ... other fields
}

# Screenshot of Kibana Discover showing the new 'geoip' field with enriched data
# Optional: Screenshot of a Kibana Map showing log events plotted by location
```


## Reflection Questions

1.  How does GeoIP enrichment enhance the analytical capabilities of your log data?
2.  What security use cases can benefit from having geographical information in your logs?
3.  What are the prerequisites for the GeoIP filter to work correctly?

## Next Steps

*   [Import a Custom Kibana Dashboard](09-custom-kibana-dashboard-import.md)
*   Further reading: [Logstash GeoIP Filter Plugin](https://www.elastic.co/guide/en/logstash/current/plugins-filter-geoip.html)

## Hints / Troubleshooting

*   **GeoIP fields not appearing**:
    *   Ensure the `source` field in the `geoip` filter (e.g., `source_ip`) actually contains a valid public IP address that can be resolved. Private IPs (like 192.168.x.x, 10.x.x.x) will not be enriched.
    *   Check Logstash logs for any errors related to GeoIP. It might indicate issues downloading the database or parsing the IP.
    *   Verify your `grok` patterns are correctly extracting the IP address into the specified field (`source_ip`).
*   **Logstash internet access**: If Logstash is in an isolated environment, you may need to manually download the GeoIP database and configure the `database` option in the `geoip` filter.
