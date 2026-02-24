---
Title: Exercise 05: Log File Parser with Python (using `re`)
Objective: Learn to parse structured and semi-structured log files using Python's built-in `re` (regular expression) module to extract relevant information, such as IP addresses, timestamps, and event details. This is a fundamental skill for log analysis and incident response.
Estimated Time: 30-60 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Create a sample log file named `access.log` in your working directory with the following content (or similar access log entries):
    ```
    192.168.1.100 - user1 [24/Feb/2026:10:00:01 +0000] "GET /index.html HTTP/1.1" 200 1024 "http://example.com/" "Mozilla/5.0"
    10.0.0.5 - - [24/Feb/2026:10:00:15 +0000] "GET /images/logo.png HTTP/1.1" 200 5120 "-" "Chrome/98.0"
    192.168.1.200 - admin [24/Feb/2026:10:00:30 +0000] "POST /login HTTP/1.1" 401 150 "-" "Mozilla/5.0"
    172.16.0.1 - user2 [24/Feb/2026:10:01:05 +0000] "GET /private/data.php HTTP/1.1" 200 2048 "http://example.com/login" "Edge/97.0"
    192.168.1.100 - user1 [24/Feb/2026:10:01:20 +0000] "GET /error HTTP/1.1" 404 200 "-" "Firefox/95.0"
    ```
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `log_parser.py`.

2.  **Import the `re` Module**
    *   Add `import re` at the beginning of your script.

3.  **Define the Regular Expression Pattern**
    *   Based on the `access.log` format, create a regular expression to capture relevant fields. For example, to capture IP, timestamp, request, status code, and bytes:
        ```python
        # Regex to capture IP, user (optional), timestamp, request method, path, HTTP version, status code, size
        log_pattern = re.compile(
            r'(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}) - (\S+) \[([^\]]+)\] "([A-Z]+)\s(/[^"]*)\s([^"]+)" (\d{3}) (\d+|-)'
        )
        ```
    *   *Explanation:*
        *   `(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})`: Captures an IPv4 address.
        *   `- (\S+)`: Captures the username (or `-` if not present).
        *   `\[([^\]]+)\]`: Captures the timestamp within square brackets.
        *   `"([A-Z]+)\s(/[^"]*)\s([^"]+)"`: Captures the request method, path, and HTTP version.
        *   `(\d{3})`: Captures the HTTP status code.
        *   `(\d+|-)`: Captures the response size (or `-`).

4.  **Read and Parse the Log File**
    *   Open `access.log` for reading.
    *   Iterate through each line of the file.
    *   For each line, use `log_pattern.search(line)` to find matches.
    *   If a match is found, extract the captured groups (e.g., `match.groups()`).

5.  **Process and Print Extracted Data**
    *   Print the extracted information in a structured way.
    *   Example: Print source IP, requested path, and HTTP status code.

## Example `log_parser.py`

```python
import re
import sys

def parse_access_log(log_file_path):
    """Parses an access log file using regex and extracts key information."""
    # Regex to capture IP, user (optional), timestamp, request method, path, HTTP version, status code, size
    # This pattern assumes a common Apache/Nginx combined log format
    log_pattern = re.compile(
        r'(\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}) - (\S+) \[([^\]]+)\] "([A-Z]+)\s(/[^"]*)\s([^"]+)" (\d{3}) (\d+|-)'
    )

    print(f"--- Parsing Log File: {log_file_path} ---")
    
    try:
        with open(log_file_path, 'r') as f:
            for line_num, line in enumerate(f, 1):
                match = log_pattern.search(line)
                if match:
                    # Extract captured groups
                    ip_address = match.group(1)
                    username = match.group(2)
                    timestamp = match.group(3)
                    request_method = match.group(4)
                    request_path = match.group(5)
                    http_version = match.group(6)
                    status_code = match.group(7)
                    response_size = match.group(8)

                    print(f"Line {line_num}:")
                    print(f"  IP: {ip_address}")
                    print(f"  User: {username if username != '-' else 'N/A'}")
                    print(f"  Time: {timestamp}")
                    print(f"  Request: {request_method} {request_path} {http_version}")
                    print(f"  Status: {status_code}")
                    print(f"  Size: {response_size}")
                    print("-" * 20)
                else:
                    print(f"Line {line_num}: No match found for: {line.strip()}")
                    print("-" * 20)
    except FileNotFoundError:
        print(f"Error: Log file '{log_file_path}' not found.")
        sys.exit(1)
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        sys.exit(1)

if __name__ == "__main__":
    log_file = "access.log" # Make sure this file exists in the same directory
    parse_access_log(log_file)
```

## Expected Output

```
--- Parsing Log File: access.log ---
Line 1:
  IP: 192.168.1.100
  User: user1
  Time: 24/Feb/2026:10:00:01 +0000
  Request: GET /index.html HTTP/1.1
  Status: 200
  Size: 1024
--------------------
Line 2:
  IP: 10.0.0.5
  User: N/A
  Time: 24/Feb/2026:10:00:15 +0000
  Request: GET /images/logo.png HTTP/1.1
  Status: 200
  Size: 5120
--------------------
Line 3:
  IP: 192.168.1.200
  User: admin
  Time: 24/Feb/2026:10:00:30 +0000
  Request: POST /login HTTP/1.1
  Status: 401
  Size: 150
--------------------
Line 4:
  IP: 172.16.0.1
  User: user2
  Time: 24/Feb/2026:10:01:05 +0000
  Request: GET /private/data.php HTTP/1.1
  Status: 200
  Size: 2048
--------------------
Line 5:
  IP: 192.168.1.100
  User: user1
  Time: 24/Feb/2026:10:01:20 +0000
  Request: GET /error HTTP/1.1
  Status: 404
  Size: 200
--------------------
```


## Reflection Questions

1.  What are the advantages of using regular expressions for parsing log files compared to simple string manipulation methods?
2.  How would you modify the regex to extract only log entries with a specific HTTP status code (e.g., 404 errors)?
3.  What are some challenges when parsing highly unstructured or inconsistent log formats, and how might Python help address them?

## Next Steps

*   [Exercise 06: Platform Process Information with Python](../python/06-platform-process-info.md)
*   Further reading: [Python `re` module documentation](https://docs.python.org/3/library/re.html) and [Regex101.com](https://regex101.com/) for testing patterns.

## Hints / Troubleshooting

*   **Regex Debugging:** Regular expressions can be tricky. Use online tools like [Regex101.com](https://regex101.com/) to build and test your patterns against sample log lines.
*   **Performance:** For very large log files (gigabytes), reading line by line is efficient. For even greater performance, consider using `re.finditer` for multiple matches on a single line or libraries optimized for log parsing.
*   **Log Format Variability:** Different applications and systems produce logs in different formats. Always adapt your regex pattern to the specific log format you are dealing with.
*   **Error Handling:** The current script has basic file not found error handling. For production, you might want more sophisticated handling for malformed lines.
