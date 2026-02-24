---
Title: Exercise 01: Basic TCP Port Scanner with Python Sockets
Objective: Write a simple Python script to scan for open TCP ports on a target host using the built-in `socket` module. This is a fundamental skill for network reconnaissance.
Estimated Time: 30-60 minutes
Tools Needed:
*   Python 3 (pre-installed on most systems)
*   A text editor (e.g., VS Code, Sublime Text, Notepad++)
Setup Instructions:
1.  Ensure Python 3 is installed. You can check by running `python --version` or `python3 --version` in your terminal.
2.  Identify a target IP address or hostname. For this exercise, use a target on your local network that you have permission to scan, or `scanme.nmap.org`. **Avoid scanning public IP addresses without explicit permission.**
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `port_scanner.py`.

2.  **Import the `socket` Module**
    *   Add `import socket` at the beginning of your script.

3.  **Define Target and Ports**
    *   Define the target IP address or hostname and a range of ports to scan.
    *   ```python
        target = "scanme.nmap.org" # Replace with your target
        port_range = range(1, 1025) # Common ports (1-1024)
        ```
    *   You can resolve a hostname to an IP address using `socket.gethostbyname(target)`.

4.  **Implement the Scanning Logic**
    *   Iterate through the `port_range`. For each port:
        *   Create a socket: `s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)`
        *   Set a timeout (optional but recommended): `s.settimeout(1)` (1 second)
        *   Attempt to connect: `result = s.connect_ex((target_ip, port))`
        *   If `result == 0`, the port is open.
        *   Close the socket: `s.close()`

5.  **Print Results**
    *   Print whether each port is open or closed.

## Example `port_scanner.py`

```python
import socket
import sys
import datetime

def scan_port(ip, port):
    """Attempts to connect to a given port on an IP address."""
    try:
        # Create a socket object
        # AF_INET specifies the address family (IPv4)
        # SOCK_STREAM specifies the socket type (TCP)
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(1)  # Set a timeout for the connection attempt

        # Attempt to connect to the target IP and port
        result = s.connect_ex((ip, port)) # connect_ex returns 0 if successful, an error indicator otherwise

        if result == 0:
            print(f"Port {port} is OPEN")
        else:
            # print(f"Port {port} is CLOSED or filtered (Error code: {result})") # Optional: print closed ports
            pass # Suppress output for closed ports to keep output clean

        s.close() # Close the socket
    except socket.gaierror:
        print("Hostname could not be resolved.")
        sys.exit()
    except socket.error:
        print("Couldn't connect to server.")
        sys.exit()
    except Exception as e:
        print(f"An error occurred: {e}")
        sys.exit()

if __name__ == "__main__":
    target = input("Enter the target IP address or hostname: ")
    try:
        target_ip = socket.gethostbyname(target) # Resolve hostname to IP
    except socket.gaierror:
        print("Hostname could not be resolved. Exiting.")
        sys.exit()

    print(f"Scanning target: {target_ip}")
    print("-" * 50)
    t1 = datetime.datetime.now()

    # Scan common ports (1-1024)
    for port in range(1, 1025):
        scan_port(target_ip, port)

    t2 = datetime.datetime.now()
    total_time = t2 - t1
    print("-" * 50)
    print(f"Scanning Completed in: {total_time}")
```

## Expected Output

```
Enter the target IP address or hostname: scanme.nmap.org
Scanning target: 45.33.32.156
--------------------------------------------------
Port 22 is OPEN
Port 80 is OPEN
Port 9929 is OPEN
Port 31337 is OPEN
--------------------------------------------------
Scanning Completed in: 0:00:03.123456
```


## Reflection Questions

1.  What is the purpose of `socket.settimeout()` in a port scanner?
2.  How does `connect_ex()` differ from `connect()` and why is it preferred for port scanning?
3.  What are some limitations of this basic port scanner compared to a tool like Nmap?

## Next Steps

*   [Exercise 02: Parse Nmap XML Output with Python](../python/02-parse-nmap-xml-lxml.md)
*   Further reading: [Python `socket` module documentation](https://docs.python.org/3/library/socket.html)

## Hints / Troubleshooting

*   **Firewalls:** Your local firewall or the target's firewall might block connection attempts. If you get no open ports, try scanning a target you know has open ports (like your router's web interface on port 80 or 443, if you have permission).
*   **Timeouts:** Increase the `settimeout()` value if you are scanning over a slow or unreliable network.
*   **Permissions:** You don't need root/administrator privileges for a basic TCP connect scan.
