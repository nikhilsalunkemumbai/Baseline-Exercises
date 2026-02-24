---
Title: Ingest CSV File with Logstash
Objective: Use Logstash to ingest structured data from a CSV file into Elasticsearch, defining fields and data types during the process.
Estimated Time: 1-2 hours
Tools Needed: Elasticsearch, Kibana, Logstash, a sample CSV file
Setup Instructions:
    - Complete Exercise 01 (Elasticsearch and Kibana running).
    - Download and extract Logstash (matching Elasticsearch version).
    - Create a sample CSV file.
---

## Steps

1.  **Download and Configure Logstash**
    *   Download Logstash from the official Elastic website.
    *   Extract the archive to a directory (e.g., `C:\elastic\logstash`).

2.  **Create a Sample CSV File**
    *   Create a file named `users.csv` (or any name you configure) in your Logstash directory or a designated data directory.
    *   Add some sample CSV data, including a header row:
        ```csv
        user_id,username,email,registration_date,status
        1,alice,alice@example.com,2023-01-15,active
        2,bob,bob@example.com,2023-02-20,inactive
        3,charlie,charlie@example.com,2023-03-01,active
        ```

3.  **Create a Logstash Configuration File**
    *   In the `C:\elastic\logstash\config` directory, create a new file named `csv_ingestion.conf`.
    *   Add the following Logstash configuration:
        ```conf
        input {
          file {
            path => "C:/elastic/logstash/users.csv" # Adjust path to your CSV file
            start_position => "beginning"
            sincedb_path => "NUL" # For Windows, use "/dev/null" for Linux to process file from beginning every time
            # For Windows, use "C:\ProgramData\logstash\sincedb" for persistent tracking
          }
        }
        filter {
          csv {
            separator => ","
            columns => ["user_id","username","email","registration_date","status"]
            skip_header => true
          }
          mutate {
            convert => {
              "user_id" => "integer"
            }
          }
          date {
            match => ["registration_date", "yyyy-MM-dd"]
            target => "registration_date"
            timezone => "UTC"
          }
          # Add default timestamp if not present
          if ![event][ingested] {
            date {
              target => "[event][ingested]"
              function => "ISO8601"
              timezone => "UTC"
            }
          }
        }
        output {
          elasticsearch {
            hosts => ["localhost:9200"]
            index => "users-%{+YYYY.MM.dd}"
            user => "elastic"
            password => "YOUR_ELASTIC_PASSWORD" # Replace with your elastic user password
            ssl_enabled => true # Use SSL/TLS for connection to Elasticsearch
            ssl_certificate_verification => false # In a real scenario, use true and provide CA certs
          }
          stdout { codec => rubydebug } # Optional: for debugging Logstash output
        }
        ```
        **Note**: Replace `YOUR_ELASTIC_PASSWORD` with the password obtained during Elasticsearch setup. Also, adjust the `path` for the CSV file. For `sincedb_path`, `NUL` is for Windows, `/dev/null` for Linux. Consider removing `sincedb_path` for production or if you want Logstash to remember its position.

4.  **Run Logstash**
    *   Open a terminal/command prompt in the Logstash directory (`C:\elastic\logstash`) and run:
        ```bash
        bin\logstash.bat -f config\csv_ingestion.conf --config.reload.automatic # For Windows
        # bin/logstash -f config/csv_ingestion.conf --config.reload.automatic # For Linux/macOS
        ```
    *   Observe the console output. You should see events being processed and sent to Elasticsearch. Logstash will continue running, monitoring the CSV file for changes if `sincedb_path` is not `NUL` or `/dev/null`.

5.  **Verify Data in Kibana**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Management" -> "Stack Management" -> "Index Patterns" under "Kibana".
    *   Create a new index pattern for `users-*`. Select `registration_date` or `event.ingested` as the time field.
    *   Go to "Analytics" -> "Discover" and select the `users-*` index pattern.
    *   You should see your CSV data ingested as documents, with `user_id` as an integer and `registration_date` as a date field.

## Expected Output

```
# Logstash console output showing events processed
[INFO ] 2023-10-27 15:30:00.123 [main] file - Received line: user_id,username,email,registration_date,status
[INFO ] 2023-10-27 15:30:00.456 [main] file - Received line: 1,alice,alice@example.com,2023-01-15,active
[INFO ] 2023-10-27 15:30:00.789 [main] file - Received line: 2,bob,bob@example.com,2023-02-20,inactive
[INFO ] 2023-10-27 15:30:01.123 [main] file - Received line: 3,charlie,charlie@example.com,2023-03-01,active
# Example of stdout debug output
{
       "user_id" => 1,
         "email" => "alice@example.com",
    "@timestamp" => "2023-10-27T10:00:01.123Z",
          "path" => "C:/elastic/logstash/users.csv",
        "status" => "active",
      "@version" => "1",
    "username" => "alice",
    "registration_date" => "2023-01-15T00:00:00.000Z"
}

# Screenshot of Kibana Discover showing CSV data
```


## Reflection Questions

1.  Describe the three main sections of a Logstash configuration file (input, filter, output) and their roles.
2.  How does the `csv` filter plugin help in processing structured data?
3.  Why is data type conversion important for effective analysis in Elasticsearch?

## Next Steps

*   [Enrich Data with Logstash GeoIP](08-enrich-data-logstash-geoip.md)
*   Further reading: [Logstash Getting Started](https://www.elastic.co/guide/en/logstash/current/getting-started-logstash.html)
*   Further reading: [Logstash CSV Filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-csv.html)

## Hints / Troubleshooting

*   **Logstash not starting**: Check the configuration file (`csv_ingestion.conf`) for syntax errors. Use `bin\logstash.bat -f config\csv_ingestion.conf --config.test_and_exit` to validate.
*   **Data not appearing in Kibana**:
    *   Ensure Logstash is running and not encountering errors.
    *   Verify the `index` name in the Elasticsearch output section matches what you're searching for in Kibana.
    *   Check your Elasticsearch logs for any indexing errors.
    *   Ensure the `user` and `password` for Elasticsearch are correct in the Logstash output.
    *   Make sure `ssl_enabled => true` and `ssl_certificate_verification => false` are set for local testing if you haven't configured proper certificates.
*   **Time field issues**: If `registration_date` isn't correctly parsed as a date, double-check the `date` filter's `match` pattern against your CSV's date format.
