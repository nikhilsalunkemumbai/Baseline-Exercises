---
Title: Exercise 03: Simple HTTP Client with Python
Objective: Learn to make basic HTTP GET requests to retrieve web content using Python's built-in `urllib.request` module. This is a foundational skill for web scraping, interacting with APIs, and checking web server status.
Estimated Time: 15-30 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Identify a target URL. For this exercise, you can use a public website like `http://example.com` or `https://www.google.com`.
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `http_client.py`.

2.  **Import `urllib.request`**
    *   Add `import urllib.request` at the beginning of your script.

3.  **Define the Target URL**
    *   Set a variable for the URL you want to fetch.
    *   ```python
        url = "http://example.com" # Or https://www.google.com
        ```

4.  **Make the Request**
    *   Use `urllib.request.urlopen(url)` to open the URL. This returns a file-like object.
    *   Read the content using `.read()` on the response object.
    *   Decode the content (usually to UTF-8) using `.decode('utf-8')`.

5.  **Print Content and Status**
    *   Print a snippet of the retrieved content.
    *   Print the HTTP status code (`response.getcode()`).
    *   Print the HTTP headers (`response.info()`).

## Example `http_client.py`

```python
import urllib.request
import urllib.error # Import for handling HTTP errors
import sys

def fetch_url_content(url):
    """Fetches content from a URL and prints status, headers, and a snippet."""
    try:
        print(f"Attempting to fetch: {url}")
        # Open the URL
        with urllib.request.urlopen(url) as response:
            # Print HTTP Status Code
            print(f"HTTP Status Code: {response.getcode()}")

            # Print some headers
            print("
--- Headers ---")
            for header, value in response.info().items():
                print(f"{header}: {value}")
            print("---------------
")

            # Read and decode the content
            content = response.read().decode('utf-8')

            # Print a snippet of the content
            print("--- Content Snippet (first 500 chars) ---")
            print(content[:500])
            print("------------------------------------------")

    except urllib.error.HTTPError as e:
        print(f"HTTP Error: {e.code} - {e.reason}")
    except urllib.error.URLError as e:
        print(f"URL Error: {e.reason}. Check network connection or URL.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")

if __name__ == "__main__":
    target_url = input("Enter the URL to fetch (e.g., http://example.com): ")
    if not target_url.startswith("http://") and not target_url.startswith("https://"):
        print("Invalid URL. Please include 'http://' or 'https://'.")
        sys.exit(1)
    fetch_url_content(target_url)
```

## Expected Output

```
Enter the URL to fetch (e.g., http://example.com): http://example.com
Attempting to fetch: http://example.com
HTTP Status Code: 200

--- Headers ---
Accept-Ranges: bytes
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Mon, 24 Feb 2026 12:00:00 GMT
Etag: "3147526947+ident"
Expires: Mon, 03 Mar 2026 12:00:00 GMT
Last-Modified: Thu, 17 Oct 2024 07:18:26 GMT
Server: ECS (dca/2437)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1256
---------------

--- Content Snippet (first 500 chars) ---
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", He
------------------------------------------
```


## Reflection Questions

1.  What is the purpose of `urllib.error.HTTPError` and `urllib.error.URLError` in the context of making HTTP requests?
2.  How would you modify this script to send an HTTP POST request instead of a GET request?
3.  What security considerations should be kept in mind when fetching content from untrusted URLs?

## Next Steps

*   [Exercise 04: File Hash Calculator with Python](../python/04-file-hash-calculator.md)
*   Further reading: [Python `urllib.request` module documentation](https://docs.python.org/3/library/urllib.request.html)

## Hints / Troubleshooting

*   **HTTPS:** `urllib.request` handles HTTPS automatically.
*   **Proxies:** For enterprise environments, you might need to configure proxy settings. You can do this by creating a `ProxyHandler` and building an `OpenerDirector`.
*   **External Libraries:** For more advanced HTTP interactions (e.g., easier POST requests, session management, custom headers), the external `requests` library is widely popular and often preferred over `urllib`.
*   **User-Agent:** Some websites block requests without a `User-Agent` header. You can add one using `request = urllib.request.Request(url, headers={'User-Agent': 'Mozilla/5.0'})` and then `urllib.request.urlopen(request)`.
