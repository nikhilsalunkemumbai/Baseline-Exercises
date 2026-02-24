---
Title: Exercise 16: JSON API Interaction with Python
Objective: Learn to interact with a JSON-based API using Python's built-in `urllib.request` for HTTP requests and `json` module for parsing JSON data. This is a crucial skill for integrating with various security tools, threat intelligence platforms, and cloud services.
Estimated Time: 30-60 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Identify a public JSON API to interact with. A good example is the "JSON Placeholder" API (`https://jsonplaceholder.typicode.com`), which provides fake online REST API for testing. We will use the `/todos` endpoint.
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `json_api_client.py`.

2.  **Import Modules**
    *   Add `import urllib.request`, `import urllib.error`, and `import json` at the beginning of your script.

3.  **Define the API Endpoint**
    *   Set the `api_url` variable to your chosen endpoint (e.g., `https://jsonplaceholder.typicode.com/todos/1` for a single todo item, or `https://jsonplaceholder.typicode.com/todos` for a list).

4.  **Make an HTTP GET Request**
    *   Use `urllib.request.urlopen(api_url)` to fetch the data.
    *   Read the response content.

5.  **Parse the JSON Response**
    *   Use `json.loads(response_content)` to parse the JSON string into a Python dictionary or list.

6.  **Process and Print Data**
    *   Access specific elements of the parsed JSON data (e.g., 'title', 'completed').
    *   Print the extracted information.

## Example `json_api_client.py`

```python
import urllib.request
import urllib.error
import json
import sys

def fetch_and_parse_json(api_url):
    """Fetches data from a JSON API and parses the response."""
    try:
        print(f"--- Fetching data from: {api_url} ---")
        # Make the HTTP GET request
        with urllib.request.urlopen(api_url) as response:
            status_code = response.getcode()
            print(f"HTTP Status Code: {status_code}")

            # Read the response body
            json_data_bytes = response.read()
            
            # Decode bytes to a string
            json_data_string = json_data_bytes.decode('utf-8')

            # Parse the JSON string into a Python object (dict or list)
            parsed_json = json.loads(json_data_string)

            print("
--- Parsed JSON Data ---")
            # For demonstration, pretty-print the entire JSON
            print(json.dumps(parsed_json, indent=4))

            # Example of accessing specific data (if it's a dictionary)
            if isinstance(parsed_json, dict) and 'title' in parsed_json:
                print(f"
Extracted Title: {parsed_json['title']}")
                print(f"Extracted Completed Status: {parsed_json['completed']}")
            elif isinstance(parsed_json, list) and parsed_json:
                print(f"
Extracted first item's title: {parsed_json[0].get('title', 'N/A')}")
                print(f"Extracted first item's completed status: {parsed_json[0].get('completed', 'N/A')}")
            
    except urllib.error.HTTPError as e:
        print(f"HTTP Error: {e.code} - {e.reason}")
    except urllib.error.URLError as e:
        print(f"URL Error: {e.reason}. Check network connection or API URL.")
    except json.JSONDecodeError as e:
        print(f"JSON Decode Error: {e}. Response was not valid JSON.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        sys.exit(1)

if __name__ == "__main__":
    # Example: Fetch a single todo item
    api_endpoint_single = "https://jsonplaceholder.typicode.com/todos/1"
    fetch_and_parse_json(api_endpoint_single)

    # Example: Fetch a list of todo items
    # api_endpoint_list = "https://jsonplaceholder.typicode.com/todos"
    # fetch_and_parse_json(api_endpoint_list)
```

## Expected Output

```
--- Fetching data from: https://jsonplaceholder.typicode.com/todos/1 ---
HTTP Status Code: 200

--- Parsed JSON Data ---
{
    "userId": 1,
    "id": 1,
    "title": "delectus aut autem",
    "completed": false
}

Extracted Title: delectus aut autem
Extracted Completed Status: false
```
![Expected output](images/exercise-16-output.png)

## Reflection Questions

1.  What is JSON, and why has it become the de facto standard for web APIs?
2.  How does the `json.loads()` function convert a JSON string into Python data structures?
3.  How can interacting with JSON APIs be utilized in security automation (e.g., querying threat intelligence feeds, managing firewall rules, or orchestrating incident response actions)?

## Next Steps

*   Proceed to the next tool set: Elastic Stack (ELK) Exercises.

## Hints / Troubleshooting

*   **Network Connectivity:** Ensure your machine has internet access to reach the API endpoint.
*   **API Rate Limits:** Be mindful of rate limits when interacting with public APIs. Repeated requests in a short period might lead to temporary bans.
*   **Authentication:** Many real-world APIs require authentication (e.g., API keys, OAuth tokens). This would typically involve sending additional headers with your request. The `urllib.request.Request` object allows you to add custom headers.
*   **External `requests` library:** For more user-friendly and feature-rich API interactions, the external `requests` library is highly recommended and widely used. It simplifies tasks like sending JSON data in POST requests and handling authentication.
