---
Title: Exercise 02: Parse Nmap XML Output with Python
Objective: Learn to parse Nmap's XML output using Python's built-in `xml.etree.ElementTree` module. This allows for automated processing and integration of Nmap scan results into other tools or reports.
Estimated Time: 45-90 minutes
Tools Needed:
*   Python 3
*   A text editor
*   Nmap (to generate the XML output)
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Generate an Nmap XML output file. Run an Nmap scan (e.g., against `scanme.nmap.org` or a local target you have permission for) and save the output in XML format:
    ```bash
    nmap -sV -O -oX nmap_scan_results.xml scanme.nmap.org
    ```
    This command performs a service version detection (`-sV`) and OS detection (`-O`), and saves the output to `nmap_scan_results.xml` (`-oX`).

---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `parse_nmap_xml.py`.

2.  **Import the `xml.etree.ElementTree` Module**
    *   Add `import xml.etree.ElementTree as ET` at the beginning of your script.

3.  **Load the XML File**
    *   Use `ET.parse()` to load your `nmap_scan_results.xml` file.

4.  **Iterate Through Hosts**
    *   Find the root element. Nmap's XML output has a `nmaprun` root.
    *   Iterate through each `host` element within the XML.

5.  **Extract Host Information**
    *   For each host, extract its IP address (`address` tag with `addrtype="ipv4"`).
    *   Extract the hostname (`hostname` tag).
    *   Determine if the host is up or down (`status` tag).

6.  **Extract Port Information**
    *   For each `host`, iterate through its `ports` and then `port` elements.
    *   Extract the port number (`portid` attribute), protocol (`protocol` attribute), state (`state` tag), and service name (`service` tag).
    *   If available, extract service version information (`service` tag attributes like `name`, `product`, `version`).

7.  **Print or Store the Extracted Data**
    *   Print the gathered information in a human-readable format.

## Example `parse_nmap_xml.py`

```python
import xml.etree.ElementTree as ET
import sys

def parse_nmap_xml(xml_file):
    """Parses an Nmap XML output file and prints key information."""
    try:
        tree = ET.parse(xml_file)
        root = tree.getroot()
    except FileNotFoundError:
        print(f"Error: XML file '{xml_file}' not found.")
        sys.exit(1)
    except ET.ParseError:
        print(f"Error: Could not parse XML file '{xml_file}'. Check its format.")
        sys.exit(1)

    print(f"--- Nmap Scan Report from '{xml_file}' ---")

    for host in root.findall('host'):
        ip_address = host.find("address[@addrtype='ipv4']").get('addr')
        hostname_elem = host.find('hostnames/hostname')
        hostname = hostname_elem.get('name') if hostname_elem is not None else "N/A"
        
        status_elem = host.find('status')
        status = status_elem.get('state') if status_elem is not None else "Unknown"

        print(f"
Host: {hostname} ({ip_address}) - Status: {status.upper()}")
        
        # Check if host is 'up' before checking for ports
        if status == 'up':
            ports = host.find('ports')
            if ports is not None:
                for port in ports.findall('port'):
                    port_id = port.get('portid')
                    protocol = port.get('protocol')
                    
                    state_elem = port.find('state')
                    state = state_elem.get('state') if state_elem is not None else "Unknown"
                    
                    service_elem = port.find('service')
                    service_name = service_elem.get('name') if service_elem is not None else "N/A"
                    service_product = service_elem.get('product') if service_elem is not None else ""
                    service_version = service_elem.get('version') if service_elem is not None else ""
                    
                    print(f"  Port: {port_id}/{protocol} | State: {state} | Service: {service_name} {service_product} {service_version}".strip())
            else:
                print("  No ports found or scanned for this host.")
        else:
            print("  Host is down, no port information available.")

    print("
--- End of Report ---")

if __name__ == "__main__":
    nmap_xml_file = "nmap_scan_results.xml" # Make sure this file exists from Nmap scan
    parse_nmap_xml(nmap_xml_file)
```

## Expected Output

```
--- Nmap Scan Report from 'nmap_scan_results.xml' ---

Host: scanme.nmap.org (45.33.32.156) - Status: UP
  Port: 22/tcp | State: open | Service: ssh OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13
  Port: 80/tcp | State: open | Service: http Apache httpd 2.4.7
  Port: 9929/tcp | State: open | Service: nping-echo
  Port: 31337/tcp | State: open | Service: Elite

--- End of Report ---
```


## Reflection Questions

1.  Why is parsing Nmap's XML output more robust for automated processing than parsing its normal text output?
2.  How would you extend this script to specifically look for hosts with a particular open port (e.g., all hosts with SSH open)?
3.  What are the advantages of using a built-in library like `xml.etree.ElementTree` over external libraries for basic XML parsing in a security tool?

## Next Steps

*   [Exercise 03: Simple HTTP Client with Python](../python/03-simple-http-client.md)
*   Further reading: [Python `xml.etree.ElementTree` documentation](https://docs.python.org/3/library/xml.etree.ElementTree.html)

## Hints / Troubleshooting

*   **XML File Not Found:** Ensure `nmap_scan_results.xml` is in the same directory as your Python script, or provide the full path to the file.
*   **Permissions:** You might need `sudo` or Administrator privileges to generate the XML file with Nmap, depending on the scan type. The Python script itself does not require special privileges to read the file.
*   **Error Handling:** Add more robust error handling (e.g., checking if `address` or `hostname` elements exist before trying to access their attributes) for more complex Nmap outputs.
*   **`lxml`:** For more advanced or faster XML parsing, consider the external `lxml` library, which is highly optimized and offers better XPath support.
