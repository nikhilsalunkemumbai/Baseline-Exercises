---
Title: Install Elasticsearch and Kibana Dev Tools
Objective: Install and configure a basic Elasticsearch and Kibana setup, and familiarize with Kibana's Dev Tools for direct interaction with Elasticsearch.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana, a web browser
Setup Instructions:
    - Download and extract Elasticsearch (version 8.x or later recommended).
    - Download and extract Kibana (matching Elasticsearch version).
    - Ensure Java is installed (Elasticsearch requires a compatible JVM).
    - A stable internet connection for downloads.
---

## Steps

1.  **Download and Start Elasticsearch**
    *   Download Elasticsearch from the official Elastic website.
    *   Extract the downloaded archive to a directory (e.g., `C:\elastic\elasticsearch`).
    *   Open a terminal/command prompt in the Elasticsearch directory (`C:\elastic\elasticsearch\bin`) and run:
        *   `elasticsearch.bat` (Windows)
        *   `./elasticsearch` (Linux/macOS)
    *   Wait for Elasticsearch to start and note the generated `elastic` user password and enrollment token (for Kibana).

2.  **Download and Start Kibana**
    *   Download Kibana from the official Elastic website, ensuring the version matches your Elasticsearch version.
    *   Extract the downloaded archive to a directory (e.g., `C:\elastic\kibana`).
    *   Open a terminal/command prompt in the Kibana directory (`C:\elastic\kibana\bin`) and run:
        *   `kibana.bat` (Windows)
        *   `./kibana` (Linux/macOS)
    *   Follow the prompts to connect Kibana to Elasticsearch using the enrollment token. Log in with the `elastic` user and password.

3.  **Access Kibana Dev Tools**
    *   Once Kibana is running, open your web browser and navigate to `http://localhost:5601`.
    *   Log in using the `elastic` user and the password noted during Elasticsearch startup.
    *   In the Kibana left navigation pane, go to "Management" -> "Dev Tools".

4.  **Verify Elasticsearch Cluster Health using Dev Tools**
    *   In the Console section of Dev Tools, type the following GET request:
        ```json
        GET /_cluster/health
        ```
    *   Click the green play button or press `Ctrl/Cmd + Enter` to execute the request.
    *   Observe the response. The `status` should ideally be `green` or `yellow`.

5.  **Create a Sample Index and Document**
    *   In Dev Tools, execute the following commands to create an index and add a document:
        ```json
        PUT /my_first_index
        {
          "settings": {
            "number_of_shards": 1,
            "number_of_replicas": 0
          }
        }

        POST /my_first_index/_doc
        {
          "message": "Hello Elastic Stack!",
          "timestamp": "2023-10-27T10:00:00Z"
        }
        ```
    *   Verify the document was indexed successfully by checking the `result` in the response.

## Expected Output

```json
# Expected output for GET /_cluster/health (status might be yellow)
{
  "cluster_name" : "elasticsearch",
  "status" : "yellow",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 1,
  "active_shards" : 1,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 1,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 50.0
}

# Expected output for POST /my_first_index/_doc
{
  "_index" : "my_first_index",
  "_id" : "...",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```
![Expected output](images/exercise-01-output.png)

## Reflection Questions

1.  What is the purpose of Elasticsearch and Kibana in the Elastic Stack?
2.  How do Dev Tools simplify interaction with Elasticsearch?
3.  What does a `yellow` cluster status indicate in Elasticsearch?

## Next Steps

*   [Ingest a Log File with Filebeat](02-ingest-log-file-beats.md)
*   Further reading: [Elasticsearch Getting Started](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started-elasticsearch.html)
*   Further reading: [Kibana Getting Started](https://www.elastic.co/guide/en/kibana/current/get-started.html)

## Hints / Troubleshooting

*   **JVM Errors**: Ensure you have a compatible Java Development Kit (JDK) installed and configured in your system's PATH. Elasticsearch 8.x bundles its own JDK, but older versions might require a separate installation.
*   **Kibana not connecting**: Verify that Elasticsearch is running and accessible. Check Kibana logs for connection errors. Ensure the enrollment token was correctly used.
*   **Firewall issues**: If running on a VM or a server, ensure ports 9200 (Elasticsearch) and 5601 (Kibana) are open.