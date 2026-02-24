---
Title: Import a Custom Kibana Dashboard
Objective: Learn how to import pre-built or custom Kibana dashboards and visualizations, which can be shared across teams or used for quick setup of common monitoring views.
Estimated Time: 1 hour
Tools Needed: Elasticsearch, Kibana, a pre-built Kibana dashboard JSON file
Setup Instructions:
    - Complete Exercise 01 (Elasticsearch and Kibana running).
    - Ensure you have some data ingested into Elasticsearch (e.g., from Filebeat, Logstash).
    - Obtain a sample Kibana dashboard JSON file. For this exercise, we will simulate one or point to a generic one.
---

## Steps

1.  **Obtain/Create a Sample Kibana Export File**
    *   **Option A (Recommended for this exercise - create your own):** Go to your existing dashboard (e.g., "Log Monitoring Overview" from Exercise 06). Click "Share" (top right) -> "Export". Select the dashboard, optionally its associated visualizations and saved searches, and export as JSON. Save this file as `my_dashboard_export.json`.
    *   **Option B (Generic Example):** For learning purposes, you can find generic dashboard exports online (e.g., for Filebeat or Nginx logs) or create a very simple one manually for demonstration. A simple one might look like this (this is illustrative, a full export is much larger):
        ```json
        {
          "objects": [
            {
              "id": "my-simple-viz",
              "type": "visualization",
              "attributes": {
                "title": "My Simple Test Visualization",
                "visState": "{"type":"metric","aggs":[{"id":"1","enabled":true,"type":"count","schema":"metric","params":{}}],"params":{"addTooltip":true,"addLegend":false,"isDonut":false,"radiusScale":1,"thresholdTextMode":"All","thresholdColorMode":"None","isFromTsvb":false,"metric":{"percentageMode":false,"useRange":false,"colorMode":"None","colorsRange":[],"labels":{"show":true,"color":"black"},"invertColors":false,"style":{"bgColor":false,"labelColor":true,"subText":"","fontSize":30,"trimHorizontal":false}},"splitArc":true,"splitRadial":true}}",
                "uiState": "{}",
                "description": "",
                "version": 1,
                "kibanaSavedObjectMeta": {
                  "searchSourceJson": "{"filter":[],"query":{"language":"kuery","query":""}}"
                }
              },
              "references": [],
              "migrationVersion": {
                "visualization": "7.10.0"
              }
            },
            {
              "id": "my-simple-dashboard",
              "type": "dashboard",
              "attributes": {
                "title": "My Simple Test Dashboard",
                "hits": 0,
                "timeRestore": false,
                "description": "",
                "kibanaSavedObjectMeta": {
                  "searchSourceJson": "{"filter":[],"query":{"language":"kuery","query":""}}"
                }
              },
              "references": [
                {
                  "name": "panel_0",
                  "type": "panelRef",
                  "id": "my-simple-viz"
                }
              ],
              "migrationVersion": {
                "dashboard": "7.10.0"
              },
              "panelsJSON": "[{"gridData":{"x":0,"y":0,"w":6,"h":4,"i":"1"},"panelRefName":"panel_0","type":"visualization","version":"7.10.0"}]"
            }
          ]
        }
        ```
        Save this content as `my_simple_dashboard.json`.

2.  **Access Kibana Stack Management**
    *   Open Kibana in your web browser (`http://localhost:5601`).
    *   Navigate to "Management" -> "Stack Management".

3.  **Import Saved Objects**
    *   Under "Kibana", click on "Saved Objects".
    *   Click the "Import" button in the top right corner.

4.  **Upload the Exported JSON File**
    *   Click "Import".
    *   Click "Upload file" and select the `my_dashboard_export.json` (or `my_simple_dashboard.json`) file you saved earlier.
    *   You might be prompted to resolve conflicts if objects with the same IDs already exist. For this exercise, you can typically choose to "Overwrite" or "Skip" as appropriate. If it's a new import, conflicts are less likely.
    *   Click "Import" to finalize.

5.  **Verify Imported Dashboard**
    *   Navigate to "Analytics" -> "Dashboard".
    *   Search for your imported dashboard by its title (e.g., "Log Monitoring Overview" or "My Simple Test Dashboard").
    *   Open the dashboard and verify that all visualizations and panels are displayed correctly. Note that imported dashboards might need index patterns to be already present in your Kibana instance.

## Expected Output

```
# Screenshot of the Kibana "Import Saved Objects" confirmation screen after successful import.
# Screenshot of the imported dashboard displaying its panels.
```
![Expected output](images/exercise-09-output.png)

## Reflection Questions

1.  What are the advantages of importing/exporting Kibana dashboards and visualizations?
2.  In what scenarios would you choose to import a dashboard rather than creating one from scratch?
3.  What potential issues might arise when importing dashboards from different Kibana versions or environments?

## Next Steps

*   [Set Up Alerting with Watcher](10-alerting-watcher.md)
*   Further reading: [Importing and Exporting Kibana Saved Objects](https://www.elastic.co/guide/en/kibana/current/managing-saved-objects.html)

## Hints / Troubleshooting

*   **Import Errors**: If you encounter errors during import, check:
    *   **Version Mismatch**: Dashboards exported from a significantly different Kibana version might not import cleanly.
    *   **Missing Index Patterns**: If the imported dashboard relies on an index pattern that doesn't exist in your Kibana, you'll need to create it first.
    *   **Corrupted JSON**: Ensure the JSON file is valid and complete.
*   **Permissions**: Ensure your Kibana user has the necessary permissions to import saved objects.
*   **Overwrite vs. Skip**: Be cautious when overwriting existing objects, especially in a production environment.
