---
Title: Use Data Views in Kibana
Objective: Explore and refine data within Kibana using Data Views, which offer a powerful way to interact with your data's fields, types, and mappings, independent of index patterns.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana, data in Elasticsearch (e.g., from Filebeat, Logstash, or Nginx logs)
Setup Instructions:
    - Complete Exercise 03 (logs visible in Kibana Discover).
    - Complete Exercise 04 (index pattern created).
    - If you completed Exercise 12, ensure Nginx logs are also ingested and an index pattern `nginx-access-*` is created.
---

## Steps

1.  **Access Kibana Data Views**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Management" -> "Stack Management".
    *   Under "Kibana", click on "Data Views" (this is the new name for "Index Patterns" in recent Kibana versions, the functionality is largely the same).

2.  **Review Existing Data Views**
    *   Observe the list of existing data views (e.g., `filebeat-*`, `users-*`, `nginx-access-*`).
    *   Click on one of your data views (e.g., `filebeat-*`) to inspect its details.
    *   Note the "Fields" tab, which shows all the fields available in the documents matched by this data view, along with their types (text, number, date, geo_point, etc.).

3.  **Explore Field Details**
    *   In the "Fields" tab, search for a specific field, e.g., `log.level`.
    *   Click on the field name to expand its details. You can see its type, format, and other properties.
    *   Notice the "Popularity" column, which indicates how frequently a field appears in your documents.

4.  **Add a Formatted Field (e.g., for Response Codes)**
    *   If you have Nginx logs (from Exercise 12), go to the `nginx-access-*` data view.
    *   Find the `nginx.access.response_code` field. It's an integer.
    *   Click on the pencil icon next to `Format`.
    *   Change the "Format" type to `Number`.
    *   Set "Decimal places" to `0`.
    *   Click "Save field". This helps in displaying the response codes cleanly.

5.  **Change Field Type (Caution Advised)**
    *   You can, in some cases, change the field type (e.g., from `string` to `number` if it was incorrectly mapped).
    *   **Caution**: Changing a field's type can lead to data loss or indexing issues if the underlying data doesn't conform. For demonstration, *do not actually save a type change unless you are sure*. This is more about understanding the capability.
    *   The primary use of Data Views is to define how Elasticsearch fields are presented and interacted with in Kibana, not to change the underlying Elasticsearch mapping directly (which typically requires reindexing).

6.  **Refresh Field List**
    *   If you've recently added new fields to your Elasticsearch indices (e.g., through a Logstash configuration change), you can click the "Refresh field list" button at the top right of the Data View detail page. This will update Kibana's understanding of the available fields.

## Expected Output

```
# Screenshot of the Kibana "Data Views" list showing your created data views.
# Screenshot of a Data View's "Fields" tab, showing details of a selected field (e.g., `log.level` or `nginx.access.response_code`).
```


## Reflection Questions

1.  How do Data Views (formerly Index Patterns) serve as the bridge between raw Elasticsearch data and Kibana's analytical capabilities?
2.  Why is understanding field types and formatting important for accurate data analysis and visualization?
3.  When would you need to refresh the field list for a data view?

## Next Steps

*   [Detect Security Events with Kibana Security](14-security-event-detection.md)
*   Further reading: [Kibana Data Views documentation](https://www.elastic.co/guide/en/kibana/current/managing-data-views.html)

## Hints / Troubleshooting

*   **Field not appearing**: If a field you expect is missing, ensure it's actually present in your Elasticsearch documents. You can check this in Discover or via the Elasticsearch `_mapping` API in Dev Tools (`GET /your_index/_mapping`).
*   **Incorrect field type**: If a field has an incorrect type (e.g., a number is mapped as text), this will affect aggregations and visualizations. Correcting this usually involves creating a new index with the correct mapping and reindexing your data.
*   **Data View vs. Index Pattern**: While "Data View" is the current term, you might still encounter "Index Pattern" in older documentation or discussions. The core concept remains the same: a pattern to match one or more Elasticsearch indices and define how their fields are displayed in Kibana.
