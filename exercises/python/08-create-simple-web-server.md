---
Title: Exercise 08: Create a Simple Web Server with Python
Objective: Learn to set up a basic HTTP web server using Python's built-in `http.server` module. This skill is useful for quickly serving static files, testing web applications, or as a component in a larger tool for delivering payloads during an engagement (in a controlled environment).
Estimated Time: 15-30 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Create a simple HTML file named `index.html` in your working directory with some content, e.g.:
    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Python Web Server Test</title>
    </head>
    <body>
        <h1>Hello from Python's Simple HTTP Server!</h1>
        <p>This is a test page served by a Python script.</p>
    </body>
    </html>
    ```
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `simple_web_server.py`.

2.  **Import Modules**
    *   Add `import http.server` and `import socketserver` at the beginning of your script.

3.  **Define Port and Handler**
    *   Choose a port number (e.g., 8000). Ensure it's not already in use.
    *   Specify the handler. For serving static files from the current directory, `http.server.SimpleHTTPRequestHandler` is sufficient.

4.  **Create and Start the Server**
    *   Use `socketserver.TCPServer` to create the server instance.
    *   Use `server.serve_forever()` to keep the server running indefinitely.

## Example `simple_web_server.py`

```python
import http.server
import socketserver
import sys

# Define the port on which the server will listen
PORT = 8000

# Specify the request handler
# SimpleHTTPRequestHandler serves files from the current directory
Handler = http.server.SimpleHTTPRequestHandler

# Create a TCP server
# The first argument is the address (empty string means all available interfaces)
# The second argument is the port
# The third argument is the request handler
try:
    with socketserver.TCPServer(("", PORT), Handler) as httpd:
        print(f"Serving HTTP on port {PORT}")
        print(f"To access, open your web browser and go to http://localhost:{PORT}")
        print("Press Ctrl+C to stop the server.")
        
        # Start the server and keep it running indefinitely
        httpd.serve_forever()
except OSError as e:
    if e.errno == 98: # Address already in use
        print(f"Error: Port {PORT} is already in use. Please choose a different port.")
    elif e.errno == 13: # Permission denied
        print(f"Error: Permission denied to use port {PORT}. Try a port above 1024 or run with sudo/Administrator.")
    else:
        print(f"An OS error occurred: {e}")
    sys.exit(1)
except KeyboardInterrupt:
    print("
Server stopped by user.")
    sys.exit(0)
except Exception as e:
    print(f"An unexpected error occurred: {e}")
    sys.exit(1)

```

## Expected Output

```
Serving HTTP on port 8000
To access, open your web browser and go to http://localhost:8000
Press Ctrl+C to stop the server.
```
*(After running the script, open `http://localhost:8000` in your web browser to see the `index.html` content. The terminal will show incoming requests.)*

```
127.0.0.1 - - [24/Feb/2026 16:30:00] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [24/Feb/2026 16:30:00] "GET /favicon.ico HTTP/1.1" 404 -
^C
Server stopped by user.
```
![Expected output](images/exercise-08-output.png)

## Reflection Questions

1.  In what scenarios could a simple HTTP server be useful for a security professional or incident responder (e.g., in a lab environment)?
2.  What are the security implications of running an HTTP server, especially in a production network?
3.  How would you extend this server to log detailed information about client requests (e.g., User-Agent, Referer headers)?

## Next Steps

*   [Exercise 09: Email Alert Script with Python](../python/09-email-alert-script.md)
*   Further reading: [Python `http.server` module documentation](https://docs.python.org/3/library/http.server.html)

## Hints / Troubleshooting

*   **Port in Use:** If you get an error like "Address already in use", try changing the `PORT` number to something else (e.g., 8001, 8080).
*   **Permissions:** On Linux/macOS, ports below 1024 (e.g., port 80) require root privileges (`sudo python simple_web_server.py`). For this exercise, using a port like 8000 is safer and doesn't require elevated privileges.
*   **Firewall:** Ensure your system's firewall is not blocking incoming connections to the chosen port. You might need to add an inbound rule for the port.
*   **Accessing from other devices:** To access the server from another device on your network, replace `localhost` with the IP address of the machine running the server.
