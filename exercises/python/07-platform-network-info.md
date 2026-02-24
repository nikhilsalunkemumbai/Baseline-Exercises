---
Title: Exercise 07: Platform Network Information with Python
Objective: Learn to retrieve and display network interface information (e.g., IP addresses, MAC addresses, network adapters) using Python's `subprocess` module to execute native operating system commands (`ipconfig` on Windows, `ifconfig` or `ip a` on Linux/macOS). This is crucial for network reconnaissance and system auditing.
Estimated Time: 30-60 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Open a terminal or command prompt.
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `network_info.py`.

2.  **Import Modules**
    *   Add `import subprocess` and `import platform` at the beginning of your script. `platform` is used to detect the operating system.

3.  **Determine OS and Command**
    *   Use `platform.system()` to check if the OS is 'Windows' or 'Linux'/'Darwin' (macOS).
    *   Set the appropriate command:
        *   Windows: `ipconfig /all`
        *   Linux: `ip a` (preferred over `ifconfig` for modern Linux)
        *   macOS: `ifconfig`

4.  **Execute the Command using `subprocess`**
    *   Use `subprocess.run()` to execute the command.
    *   Capture the output using `capture_output=True` and `text=True` to get output as a string.
    *   Handle potential errors (`check=True` will raise an exception for non-zero exit codes).

5.  **Parse and Display Output**
    *   Print the raw output. For a more advanced exercise, you could parse this output using regular expressions to extract specific details like IPv4 addresses, subnet masks, or MAC addresses for each adapter. For this exercise, displaying the raw output is sufficient to understand the command's utility.

## Example `network_info.py`

```python
import subprocess
import platform
import sys

def get_network_info():
    """Executes OS-specific commands to get network interface information."""
    os_name = platform.system()
    command = []

    if os_name == "Windows":
        command = ["ipconfig", "/all"]
        print("--- Windows Network Configuration (ipconfig /all) ---")
    elif os_name == "Linux":
        command = ["ip", "a"] # ip a is preferred on modern Linux over ifconfig
        print("--- Linux Network Configuration (ip a) ---")
    elif os_name == "Darwin": # macOS
        command = ["ifconfig"]
        print("--- macOS Network Configuration (ifconfig) ---")
    else:
        print(f"Unsupported operating system: {os_name}")
        sys.exit(1)

    try:
        # Run the command
        result = subprocess.run(command, capture_output=True, text=True, check=True)
        print(result.stdout)
    except FileNotFoundError:
        print(f"Error: Command '{command[0]}' not found. Is it in your PATH?")
        sys.exit(1)
    except subprocess.CalledProcessError as e:
        print(f"Error executing command: {e}")
        print(f"Stderr: {e.stderr}")
        sys.exit(1)
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        sys.exit(1)

if __name__ == "__main__":
    get_network_info()
```

## Expected Output (Windows)

```
--- Windows Network Configuration (ipconfig /all) ---

Windows IP Configuration

   Host Name . . . . . . . . . . . . : MYCOMPUTER
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No

Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Realtek PCIe GbE Family Controller
   Physical Address. . . . . . . . . : 00-11-22-33-44-55
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv4 Address. . . . . . . . . . . : 192.168.1.100(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   ...
```
## Expected Output (Linux)

```
--- Linux Network Configuration (ip a) ---
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:01:23:45 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.101/24 brd 192.168.1.255 scope global dynamic eth0
       valid_lft 86178sec preferred_lft 86178sec
    inet6 fe80::a00:27ff:fe01:2345/64 scope link 
       valid_lft forever preferred_lft forever
```
![Expected output](images/exercise-07-output-win.png) *(Example for Windows)*

## Reflection Questions

1.  Why is `ipconfig /all` or `ip a` more useful than just `ipconfig` or `ifconfig` for gathering network information?
2.  How can an incident responder use the output of these commands during a network investigation?
3.  What are the challenges of parsing the output of these commands programmatically, given their varying formats across OS versions?

## Next Steps

*   [Exercise 08: Create a Simple Web Server with Python](../python/08-create-simple-web-server.md)
*   Further reading: [Python `subprocess` module documentation](https://docs.python.org/3/library/subprocess.html) and [Python `platform` module documentation](https://docs.python.org/3/library/platform.html)

## Hints / Troubleshooting

*   **Command Variations:** Be aware that command outputs can vary slightly between different versions of the same operating system.
*   **Permissions:** `ipconfig`, `ifconfig`, and `ip a` typically do not require administrative privileges for basic information.
*   **Parsing:** For extracting specific details, you would typically use regular expressions (from `re` module) to parse the output string.
*   **External Libraries:** For more robust and OS-independent network information retrieval, the external `psutil` library is highly recommended, as it abstracts away OS-specific command parsing.
