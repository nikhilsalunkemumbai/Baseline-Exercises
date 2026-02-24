---
Title: Basic Elasticsearch Query DSL
Objective: Learn to write fundamental queries using Elasticsearch's Query DSL in Kibana Dev Tools to precisely search and retrieve documents.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana
Setup Instructions:
    - Complete Exercise 01 (Elasticsearch and Kibana running).
    - Have some data ingested into Elasticsearch (e.g., from Filebeat, Logstash, Nginx logs, or security events).
---

## Steps

1.  **Access Kibana Dev Tools**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Management" -> "Dev Tools".

2.  **Match All Query**
    *   This query retrieves all documents from a specified index.
    *   Execute the following:
        ```json
        GET /filebeat-*/_search
        {
          "query": {
            "match_all": {}
          }
        }
        ```
    *   Observe the `hits` array in the response, which contains the documents.

3.  **Match Query (Full-Text Search)**
    *   The `match` query performs a full-text search on a field.
    *   Execute the following (replace `message` and `error` with a field and keyword from your data):
        ```json
        GET /filebeat-*/_search
        {
          "query": {
            "match": {
              "message": "error"
            }
          }
        }
        ```
    *   Try with a different field and value from your Nginx logs (e.g., `nginx.access.method: "POST"`).

4.  **Term Query (Exact Match)**
    *   The `term` query is used for finding documents that contain the exact term in a field, typically used for non-analyzed fields (e.g., keywords, numbers, dates).
    *   Execute the following (replace `log.level.keyword` and `ERROR` with appropriate field/value):
        ```json
        GET /filebeat-*/_search
        {
          "query": {
            "term": {
              "log.level.keyword": "ERROR"
            }
          }
        }
        ```
    *   **Note**: For exact matches on string fields, it's often best to use the `.keyword` version of the field if available, which holds the unanalyzed string.

5.  **Range Query**
    *   The `range` query finds documents where a field's value falls within a specified range.
    *   Execute the following to find Nginx logs with response codes between 400 and 499:
        ```json
        GET /nginx-access-*/_search
        {
          "query": {
            "range": {
              "nginx.access.response_code": {
                "gte": 400,
                "lt": 500
              }
            }
          }
        }
        ```
    *   `gte`: greater than or equal to, `gt`: greater than, `lte`: less than or equal to, `lt`: less than.

6.  **Boolean Query (Combine Multiple Queries)**
    *   The `bool` query combines multiple queries using boolean clauses (`must`, `filter`, `should`, `must_not`).
    *   Execute the following to find ERROR messages from a specific IP:
        ```json
        GET /filebeat-*/_search
        {
          "query": {
            "bool": {
              "must": [
                { "match": { "log.level": "ERROR" } },
                { "term": { "source.ip": "10.0.0.5" } } # Replace with an IP from your data
              ]
            }
          }
        }
        ```
    *   Try using `filter` instead of `must` for performance if you only need exact matches (filter queries are faster as they are not scored).
    *   Experiment with `should` (OR logic) and `must_not` (NOT logic).

## Expected Output

```json
# Example response for a match_all query (truncated)
{
  "took" : ...,
  "timed_out" : false,
  "_shards" : { ... },
  "hits" : {
    "total" : {
      "value" : 10000,
      "relation" : "gte"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "filebeat-...",
        "_id" : "...",
        "_score" : 1.0,
        "_source" : {
          "message" : "...",
          "log.level" : "INFO",
          ...
        }
      },
      ...
    ]
  }
}

# Example response for a boolean query (truncated)
```
![Expected output](images/exercise-15-output.png)

## Reflection Questions

1.  What is the key difference between a `match` query and a `term` query, and when would you use each?
2.  How does the `bool` query provide flexibility in constructing complex search criteria?
3.  Why is understanding Query DSL important for advanced Elastic Stack users, even with Kibana's UI?

## Next Steps

*   [Ingest Syslog Data with Beats](16-ingest-syslog-beats.md)
*   Further reading: [Elasticsearch Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)

## Hints / Troubleshooting

*   **No results**:
    *   Ensure the index name is correct (e.g., `filebeat-*`, `nginx-access-*`).
    *   Verify the field names in your queries match your actual data (check mapping in Dev Tools: `GET /your_index/_mapping`).
    *   Check your data in Kibana Discover to confirm the existence of the terms you're searching for.
*   **Syntax errors**: Query DSL is JSON-based. Pay close attention to commas, brackets, and curly braces. Kibana Dev Tools usually provides helpful syntax highlighting and error messages.
*   **Case sensitivity**: `term` queries are case-sensitive. `match` queries are typically not, as they involve text analysis.
