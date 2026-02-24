---
Title: Exercise 14: Regex for IOC Extraction with Python
Objective: Learn to extract Indicators of Compromise (IOCs) such as IP addresses, URLs, and email addresses from text data using Python's `re` (regular expression) module. This is a crucial skill for threat intelligence, log analysis, and incident response.
Estimated Time: 30-60 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Have a sample text file or string containing various potential IOCs. For example, create `sample_data.txt` with the following content:
    ```
    Analysis Report 2026-02-24
    Threat Actor: APT-XYZ
    Malicious IP: 192.168.1.10, 10.0.0.5, 172.16.20.30
    Command & Control Server: http://malicious-c2.com/payload.exe, https://evil.phishing.org/login
    Phishing attempt from: badguy@example.net, support@legitsite.com
    Referer: http://www.legitsite.com/page?param=value
    Detected payload download from 1.2.3.4 (another malicious IP)
    Suspicious domain: example.xyz
    ```
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `ioc_extractor.py`.

2.  **Import the `re` Module**
    *   Add `import re` at the beginning of your script.

3.  **Define Regular Expressions for Different IOCs**
    *   **IPv4 Address:** `r'\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b'`
    *   **URL:** `r'https?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+'` (a more robust regex is often needed for all cases, but this is a good start)
    *   **Email Address:** `r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b'`
    *   **Domain:** `r'\b(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?\.)+[a-zA-Z]{2,6}\b'` (can be refined to exclude IPs)

4.  **Read Input Data**
    *   Read the content of `sample_data.txt` into a string.

5.  **Extract IOCs using `re.findall()`**
    *   For each regex, use `re.findall(pattern, text)` to get all non-overlapping matches.
    *   `re.findall()` returns a list of all matches.

6.  **Print the Extracted IOCs**
    *   Print each type of extracted IOC. Consider using `set()` to get unique IOCs.

## Example `ioc_extractor.py`

```python
import re
import sys

def extract_iocs(text_data):
    """
    Extracts common Indicators of Compromise (IOCs) from text data.
    """
    
    # Regex for IPv4 addresses
    # \b ensures whole word match, avoiding parts of larger numbers
    ip_pattern = re.compile(r'\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b')
    
    # Regex for URLs (HTTP/HTTPS) - simplified, more robust ones exist
    url_pattern = re.compile(r'https?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+')
    
    # Regex for Email Addresses
    email_pattern = re.compile(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b')

    # Regex for Domains - needs care to avoid matching IPs.
    # Excludes patterns that look like IP addresses
    domain_pattern = re.compile(r'\b(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?\.)+[a-zA-Z]{2,6}\b(?<!\d{1,3}\.\d{1,3}\.\d{1,3})') # Negative lookbehind to exclude IPs
    
    # Extract IOCs
    ips = set(ip_pattern.findall(text_data))
    urls = set(url_pattern.findall(text_data))
    emails = set(email_pattern.findall(text_data))
    # Extract domains and remove any IPs that might have been partially matched
    domains = set([d for d in domain_pattern.findall(text_data) if not re.match(r'\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}', d)])


    print("--- Extracted IOCs ---")
    print("
IPv4 Addresses:")
    if ips:
        for ip in sorted(list(ips)):
            print(f"  - {ip}")
    else:
        print("  No IPv4 addresses found.")

    print("
URLs:")
    if urls:
        for url in sorted(list(urls)):
            print(f"  - {url}")
    else:
        print("  No URLs found.")

    print("
Email Addresses:")
    if emails:
        for email in sorted(list(emails)):
            print(f"  - {email}")
    else:
        print("  No email addresses found.")

    print("
Domains (excluding IPs):")
    if domains:
        for domain in sorted(list(domains)):
            print(f"  - {domain}")
    else:
        print("  No domains found.")
    print("----------------------")


if __name__ == "__main__":
    sample_data_file = "sample_data.txt"
    try:
        with open(sample_data_file, 'r', encoding='utf-8') as f:
            data = f.read()
        extract_iocs(data)
    except FileNotFoundError:
        print(f"Error: Sample data file '{sample_data_file}' not found.")
        sys.exit(1)
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        sys.exit(1)
```

## Expected Output

```
--- Extracted IOCs ---

IPv4 Addresses:
  - 1.2.3.4
  - 10.0.0.5
  - 172.16.20.30
  - 192.168.1.10

URLs:
  - http://malicious-c2.com/payload.exe
  - http://www.legitsite.com/page?param=value
  - https://evil.phishing.org/login

Email Addresses:
  - badguy@example.net
  - support@legitsite.com

Domains (excluding IPs):
  - example.net
  - example.xyz
  - malicious-c2.com
  - phishing.org
  - legitsite.com

----------------------
```
![Expected output](images/exercise-14-output.png)

## Reflection Questions

1.  Why is it important to use `\b` (word boundary) in regular expressions for extracting IOCs like IP addresses or email addresses?
2.  What are the challenges in creating a "perfect" regular expression for complex IOCs like URLs, and why might you choose a simpler one?
3.  How can an automated IOC extraction script be integrated into a Security Operations Center (SOC) workflow?

## Next Steps

*   [Exercise 15: Data Encoding/Decoding with Python](../python/15-data-encoding-decoding.md)
*   Further reading: [Python `re` module documentation](https://docs.python.org/3/library/re.html) and [Common IOC Regex patterns](https://www.google.com/search?q=common+ioc+regex+patterns)

## Hints / Troubleshooting

*   **Regex Complexity:** Regular expressions can be very powerful but also complex and hard to debug. Test your patterns thoroughly with various valid and invalid inputs.
*   **False Positives/Negatives:** Be aware that simple regex patterns might produce false positives (match things that aren't IOCs) or false negatives (miss actual IOCs). Refine your patterns as needed.
*   **Performance:** For very large text inputs, compiling the regex patterns (`re.compile()`) once can improve performance for repeated searches.
*   **Domain Regex:** The domain regex is tricky because a valid domain name can look like part of an IP address. The negative lookbehind `(?<!\d{1,3}\.\d{1,3}\.\d{1,3})` helps to avoid matching parts of IP addresses as domains.
