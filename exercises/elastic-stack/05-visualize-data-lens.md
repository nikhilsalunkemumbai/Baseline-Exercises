---
Title: Visualize Data with Kibana Lens
Objective: Create a basic visualization, such as a bar chart of log levels, using Kibana Lens to understand data distribution and trends.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana
Setup Instructions:
    - Complete Exercise 03 (logs visible in Kibana Discover).
    - Complete Exercise 04 (index pattern created).
    - Ensure you have some log data ingested with varying `log.level` values (e.g., INFO, WARNING, ERROR).
---

## Steps

1.  **Access Kibana Visualize Library**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Analytics" -> "Visualize Library".

2.  **Create a New Visualization with Lens**
    *   Click "Create visualization".
    *   Select "Lens".

3.  **Choose Your Data Source**
    *   In the Lens editor, select the `filebeat-*` index pattern (or your relevant index pattern) from the dropdown.

4.  **Create a Bar Chart of Log Levels**
    *   Drag and drop the `log.level` field from the "Fields" list on the left to the "X-axis" drop zone in the center. Lens will automatically suggest a bar chart.
    *   Drag and drop the `Count` (or `Number of records`) metric from the "Fields" list to the "Y-axis" drop zone.
    *   Observe the bar chart showing the distribution of your log levels (INFO, WARNING, ERROR).

5.  **Customize the Visualization**
    *   **Change Chart Type**: In the "Chart Type" dropdown (top right), experiment with other types like "Donut chart" or "Area chart" to see how your data can be represented differently. Switch back to "Bar chart".
    *   **Add Filtering**: In the "Filters" section, add a filter, e.g., `log.level is one of ERROR`. Observe how the chart updates. Remove the filter.
    *   **Sort Bars**: On the X-axis configuration, click on "Advanced options" and change the sort order if desired (e.g., sort by `Count (descending)`).

6.  **Save the Visualization**
    *   Click the "Save" button in the top right corner.
    *   Give your visualization a meaningful title, e.g., "Log Levels Distribution".
    *   Click "Save".

## Expected Output

```
# Screenshot of Kibana Lens showing a bar chart of log levels (e.g., INFO, WARNING, ERROR)
```


## Reflection Questions

1.  How does Kibana Lens simplify the process of creating visualizations compared to traditional methods?
2.  What insights can you gain from visualizing log levels in a bar chart?
3.  When would you choose a different chart type (e.g., donut chart, area chart) over a bar chart for this data?

## Next Steps

*   [Build a Kibana Dashboard](06-build-dashboard.md)
*   Further reading: [Kibana Lens documentation](https://www.elastic.co/guide/en/kibana/current/lens.html)

## Hints / Troubleshooting

*   **No data visible**: Ensure your time range in Kibana (top right) encompasses the time period of your ingested logs.
*   **Field not appearing**: If `log.level` (or another expected field) does not appear in the "Fields" list, verify that it exists in your index pattern and that your data actually contains that field. You might need to refresh your index pattern in Stack Management.
*   **Wrong chart type**: If Lens doesn't automatically pick a bar chart, you can manually select it from the "Chart Type" dropdown.
