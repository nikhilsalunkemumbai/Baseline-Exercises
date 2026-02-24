---
Title: Build a Kibana Dashboard
Objective: Create a Kibana dashboard that combines multiple saved searches and visualizations to provide a comprehensive overview of your log data.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana
Setup Instructions:
    - Complete Exercise 03 (logs visible in Kibana Discover).
    - Complete Exercise 05 (created and saved a Kibana Lens visualization).
    - Optionally, save a search from Exercise 03 (e.g., "All Errors").
---

## Steps

1.  **Access Kibana Dashboard**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Analytics" -> "Dashboard".

2.  **Create a New Dashboard**
    *   Click "Create dashboard".

3.  **Add Saved Visualizations and Searches**
    *   Click the "Add" button in the top toolbar.
    *   In the "Add panels" flyout, you will see a list of your saved visualizations and searches.
    *   Find and click on the "Log Levels Distribution" visualization you created in Exercise 05. It will be added to your dashboard.
    *   If you saved a search (e.g., "All Errors") from Exercise 03, find and add that as well.
    *   You can also add a new visualization directly from the "Add panels" flyout by clicking "Create new visualization" and going through the Lens process again. For this exercise, stick to adding existing ones.
    *   Close the "Add panels" flyout.

4.  **Arrange and Resize Panels**
    *   Click and drag the panels on the dashboard to rearrange them.
    *   Hover over the bottom-right corner of a panel and drag to resize it.
    *   Aim for a clean and informative layout.

5.  **Save the Dashboard**
    *   Click the "Save" button in the top toolbar.
    *   Give your dashboard a meaningful title, e.g., "Log Monitoring Overview".
    *   Add an optional description and tags.
    *   Click "Save".

6.  **Interact with the Dashboard**
    *   Change the time range in the top right corner (e.g., to "Last 15 minutes"). Observe how all panels update.
    *   Click on a segment of a visualization (e.g., the "ERROR" bar in your log level chart). This will create a filter that applies to the entire dashboard.
    *   Remove filters by clicking on them in the filter bar at the top.

## Expected Output

```
# Screenshot of a Kibana Dashboard showing at least two panels (e.g., "Log Levels Distribution" chart and a saved search with error logs)
```
![Expected output](images/exercise-06-output.png)

## Reflection Questions

1.  What are the benefits of combining multiple visualizations and searches into a single dashboard?
2.  How does the time range selector impact the data displayed across all panels?
3.  How can interactive filters on a dashboard help in threat hunting or incident response?

## Next Steps

*   [Ingest CSV File with Logstash](07-ingest-csv-logstash.md)
*   Further reading: [Kibana Dashboards documentation](https://www.elastic.co/guide/en/kibana/current/dashboard.html)

## Hints / Troubleshooting

*   **Panels not updating**: Ensure all visualizations and saved searches on your dashboard use the same time field in their respective index patterns.
*   **Empty panels**: Verify that the underlying visualizations and searches have data for the selected time range. Check individual visualizations in the "Visualize Library" to ensure they are working correctly.
*   **Overlapping panels**: Use the drag-and-drop and resize features carefully to avoid panels overlapping each other.
