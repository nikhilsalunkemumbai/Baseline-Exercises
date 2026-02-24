---
Title: Search Data in Kibana Discover
Objective: Utilize Kibana's Discover interface to perform basic searches, filter data, and understand log events.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana
Setup Instructions:
    - Complete Exercise 02 (Filebeat ingesting logs into Elasticsearch, visible in Kibana Discover).
    - Ensure you have some log data ingested (e.g., from `filebeat-*` index pattern).
---

## Steps

1.  **Access Kibana Discover**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Analytics" -> "Discover".
    *   Ensure the `filebeat-*` index pattern is selected (or the index pattern where your logs are being ingested).
    *   Adjust the time range to encompass your log data (e.g., "Last 15 minutes", "Last 1 hour", or "Today").

2.  **Perform Basic Text Search**
    *   In the search bar at the top, type a simple keyword to search for, e.g., `ERROR`.
    *   Press `Enter` or click the search icon.
    *   Observe how the displayed documents are filtered to show only those containing the keyword "ERROR".

3.  **Search with Field-Specific Queries**
    *   Clear the previous search.
    *   Search for logs containing a specific field value, e.g., `message: "Disk full"`.
    *   Observe the results, which should only show documents where the `message` field contains "Disk full".

4.  **Combine Search Queries**
    *   Clear the previous search.
    *   Combine multiple search terms using boolean operators (`AND`, `OR`, `NOT`).
    *   Try `ERROR AND message: "Disk full"`.
    *   Try `WARNING OR INFO`.
    *   Try `message: "logged in" NOT user: "guest"`.

5.  **Filter by Field Value from Document Table**
    *   In the document table, expand one of the log entries.
    *   Hover over a field (e.g., `event.severity` or `log.level`).
    *   Click the "+" icon next to a field value to add it as a filter (e.g., filter for `log.level: ERROR`).
    *   Click the "-" icon to exclude that value.
    *   Click the "pencil" icon to edit the filter.

6.  **Save a Search Query**
    *   Perform a useful search query (e.g., `log.level: ERROR AND NOT user: "admin"`).
    *   Click "Save" at the top of the Discover page.
    *   Give your search a descriptive name and save it.
    *   This saved search can be loaded later or used in dashboards.

## Expected Output

```
# Screenshot of Kibana Discover with search results for "ERROR"
```
![Expected output](images/exercise-03-output.png)

## Reflection Questions

1.  What is the advantage of using field-specific queries over simple text searches?
2.  How do boolean operators enhance your search capabilities in Kibana Discover?
3.  Why is it useful to save frequently used search queries?

## Next Steps

*   [Create an Index Pattern](04-create-index-pattern.md)
*   Further reading: [Kibana Discover documentation](https://www.elastic.co/guide/en/kibana/current/discover.html)
*   Further reading: [Kibana Query Language (KQL)](https://www.elastic.co/guide/en/kibana/current/kuery-query.html)

## Hints / Troubleshooting

*   **No results**: Ensure your time range is wide enough to include the data you're searching for.
*   **Incorrect field names**: Kibana search is case-sensitive for field names. Double-check the exact field name by expanding a document.
*   **KQL vs Lucene**: Kibana's default query language is KQL. If you are more familiar with Lucene syntax, you can switch the query language in the search bar options.
