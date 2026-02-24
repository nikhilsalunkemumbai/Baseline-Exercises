---
Title: Create an Index Pattern
Objective: Learn how to create and manage index patterns in Kibana, which are essential for visualizing and analyzing data from various Elasticsearch indices.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana
Setup Instructions:
    - Complete Exercise 02 (Filebeat ingesting logs into Elasticsearch).
    - Ensure you have data in at least one index (e.g., `filebeat-*`).
---

## Steps

1.  **Access Stack Management**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Management" -> "Stack Management".

2.  **Navigate to Index Patterns**
    *   In the Stack Management section, click on "Index Patterns" under "Kibana".

3.  **Create a New Index Pattern**
    *   Click the "Create index pattern" button.
    *   In the "Index pattern name" field, type `filebeat-*`.
        *   Observe the "Your index pattern can match X indices" message below, confirming it matches your Filebeat data.
    *   Click "Next step".
    *   From the "Time field" dropdown, select `@timestamp`. This field is crucial for time-based data filtering and visualizations.
    *   Click "Create index pattern".

4.  **Explore the Index Pattern Fields**
    *   After creation, you will see a list of all fields discovered in the indices matched by your `filebeat-*` pattern.
    *   Observe the field names, types, and whether they are "searchable", "aggregatable", and "indexed".
    *   You can refresh the field list if new fields are added to your data.

5.  **Set a Default Index Pattern (Optional)**
    *   If you have multiple index patterns, you can set one as the default. The default index pattern is automatically selected when you open Discover.
    *   Next to your `filebeat-*` index pattern in the list, click the star icon to set it as default.

## Expected Output

```
# Screenshot of the "Create index pattern" screen with "filebeat-*" entered and matches shown
# Screenshot of the Index Pattern details page showing various fields and their properties
```


## Reflection Questions

1.  What is the primary purpose of an index pattern in Kibana?
2.  Why is it important to select a "time field" when creating an index pattern?
3.  How do index patterns contribute to data organization and analysis in Kibana?

## Next Steps

*   [Visualize Data with Kibana Lens](05-visualize-data-lens.md)
*   Further reading: [Kibana Index Patterns documentation](https://www.elastic.co/guide/en/kibana/current/index-patterns.html)

## Hints / Troubleshooting

*   **No matching indices**: If you type an index pattern and it says "Your index pattern does not match any indices", ensure that data has actually been ingested into Elasticsearch and that your index pattern correctly reflects the names of those indices (e.g., `my-data-*` if your indices are `my-data-2023-10-27`).
*   **No time field**: If your data doesn't have a recognizable time field (like `@timestamp`), you might need to process it through Logstash or an ingest pipeline to add one before it can be effectively used for time-series analysis in Kibana.
