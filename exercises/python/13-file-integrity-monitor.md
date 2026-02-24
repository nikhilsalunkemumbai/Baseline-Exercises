---
Title: Exercise 13: File Integrity Monitor with Python
Objective: Develop a Python script that calculates and stores file hashes to monitor for unauthorized changes in critical files. This is a fundamental concept in cybersecurity for detecting tampering and maintaining system integrity.
Estimated Time: 45-90 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Create a directory named `monitor_files` in your working directory.
3.  Inside `monitor_files`, create a few sample text files (e.g., `config.txt`, `script.py`, `important_doc.txt`) with some initial content.
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `file_integrity_monitor.py`.

2.  **Import Modules**
    *   Add `import hashlib`, `import os`, `import json`, and `import datetime` at the beginning of your script.

3.  **Define a Hash Calculation Function**
    *   Re-use or adapt the `calculate_sha256` function from Exercise 04.

4.  **Baseline Function**
    *   Create a function `create_baseline(directory, output_file)`.
    *   It should iterate through all files in the specified `directory` (and its subdirectories).
    *   For each file, calculate its SHA256 hash.
    *   Store the file path and its hash in a dictionary.
    *   Save this dictionary as a JSON file (`output_file`).

5.  **Monitor Function**
    *   Create a function `monitor_changes(directory, baseline_file)`.
    *   Load the `baseline_file` (JSON) to get the known hashes.
    *   Iterate through all current files in the `directory`.
    *   For each current file:
        *   Calculate its current hash.
        *   Compare with the hash in the baseline.
        *   Report if the file is new, modified, or deleted.
    *   Report any files present in the baseline but not found in the current directory (deleted).

## Example `file_integrity_monitor.py`

```python
import hashlib
import os
import json
import datetime
import sys

# Configuration
MONITOR_DIR = "monitor_files"
BASELINE_FILE = "baseline.json"

def calculate_sha256(filepath):
    """Calculates the SHA256 hash of a file."""
    hasher = hashlib.sha256()
    try:
        with open(filepath, 'rb') as file:
            while True:
                chunk = file.read(4096)  # Read in 4KB chunks
                if not chunk:
                    break
                hasher.update(chunk)
        return hasher.hexdigest()
    except FileNotFoundError:
        return None # File might have been deleted
    except IOError as e:
        print(f"Error reading file '{filepath}': {e}")
        return None

def create_baseline(directory):
    """
    Creates a baseline of file hashes for all files in the specified directory
    and saves it to BASELINE_FILE.
    """
    print(f"Creating baseline for '{directory}'...")
    baseline_data = {
        "timestamp": datetime.datetime.now().isoformat(),
        "files": {}
    }
    
    if not os.path.isdir(directory):
        print(f"Error: Directory '{directory}' does not exist.")
        sys.exit(1)

    for root, _, files in os.walk(directory):
        for filename in files:
            filepath = os.path.join(root, filename)
            sha256_hash = calculate_sha256(filepath)
            if sha256_hash:
                # Store relative path for consistency
                relative_filepath = os.path.relpath(filepath, directory)
                baseline_data["files"][relative_filepath] = sha256_hash
    
    try:
        with open(BASELINE_FILE, 'w') as f:
            json.dump(baseline_data, f, indent=4)
        print(f"Baseline created and saved to '{BASELINE_FILE}'.")
    except IOError as e:
        print(f"Error saving baseline to '{BASELINE_FILE}': {e}")
        sys.exit(1)

def monitor_changes(directory):
    """
    Monitors the specified directory for changes by comparing current hashes
    against the stored baseline.
    """
    print(f"Monitoring '{directory}' for changes...")

    if not os.path.exists(BASELINE_FILE):
        print(f"Error: Baseline file '{BASELINE_FILE}' not found. Please create a baseline first.")
        sys.exit(1)
    
    if not os.path.isdir(directory):
        print(f"Error: Directory '{directory}' does not exist.")
        sys.exit(1)

    try:
        with open(BASELINE_FILE, 'r') as f:
            baseline = json.load(f)
        known_files = baseline["files"]
    except json.JSONDecodeError:
        print(f"Error: Could not parse '{BASELINE_FILE}'. Is it a valid JSON file?")
        sys.exit(1)
    except IOError as e:
        print(f"Error loading baseline from '{BASELINE_FILE}': {e}")
        sys.exit(1)

    current_files = {}
    for root, _, files in os.walk(directory):
        for filename in files:
            filepath = os.path.join(root, filename)
            relative_filepath = os.path.relpath(filepath, directory)
            current_files[relative_filepath] = calculate_sha256(filepath)

    print("
--- Integrity Report ---")
    
    modified_count = 0
    new_count = 0
    deleted_count = 0

    # Check for modified and new files
    for filepath, current_hash in current_files.items():
        if filepath in known_files:
            if current_hash != known_files[filepath]:
                print(f"[MODIFIED] {filepath} (Hash changed)")
                modified_count += 1
        else:
            print(f"[NEW FILE] {filepath}")
            new_count += 1
            
    # Check for deleted files
    for filepath in known_files:
        if filepath not in current_files:
            print(f"[DELETED] {filepath}")
            deleted_count += 1

    if modified_count == 0 and new_count == 0 and deleted_count == 0:
        print("No integrity changes detected.")
    else:
        print(f"
Summary: {new_count} new, {modified_count} modified, {deleted_count} deleted.")
    print("------------------------")

if __name__ == "__main__":
    if not os.path.exists(MONITOR_DIR):
        os.makedirs(MONITOR_DIR)
        print(f"Created directory: {MONITOR_DIR}")
    
    # Example usage:
    # 1. Create a baseline
    create_baseline(MONITOR_DIR)

    # 2. Make some changes (e.g., modify a file, add a file, delete a file) in MONITOR_DIR
    #    For example, uncomment these lines to simulate changes
    # with open(os.path.join(MONITOR_DIR, "config.txt"), "a") as f:
    #     f.write("
new_setting=value")
    # with open(os.path.join(MONITOR_DIR, "new_script.py"), "w") as f:
    #     f.write("print('Hello new script')")
    # if os.path.exists(os.path.join(MONITOR_DIR, "important_doc.txt")):
    #     os.remove(os.path.join(MONITOR_DIR, "important_doc.txt"))

    input("
Press Enter to monitor for changes (after you've made some manual changes in 'monitor_files' if desired)...")
    
    # 3. Monitor for changes
    monitor_changes(MONITOR_DIR)
```

## Expected Output (Example after modifying one file and adding one)

```
Creating baseline for 'monitor_files'...
Baseline created and saved to 'baseline.json'.

Press Enter to monitor for changes (after you've made some manual changes in 'monitor_files' if desired)...

Monitoring 'monitor_files' for changes...

--- Integrity Report ---
[MODIFIED] config.txt (Hash changed)
[NEW FILE] new_script.py
------------------------
Summary: 1 new, 1 modified, 0 deleted.
```
*(You will need to manually create `monitor_files/config.txt` and `monitor_files/script.py` (or similar) before running, and then modify `config.txt` and add `new_script.py` before pressing Enter for the monitoring step.)*


## Reflection Questions

1.  Why is file hashing a more reliable method for detecting file changes than simply checking file modification times?
2.  What are the limitations of a hash-based file integrity monitor, and how can they be overcome in a production environment?
3.  How does this script lay the foundation for more advanced security tools like Host-based Intrusion Detection Systems (HIDS)?

## Next Steps

*   [Exercise 14: Regex for IOC Extraction with Python](../python/14-regex-for-ioc-extraction.md)
*   Further reading: [NIST SP 800-92 Guide to Computer Security Log Management (discusses FIM)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-92.pdf)

## Hints / Troubleshooting

*   **Initial Setup:** Ensure the `monitor_files` directory and some sample files are created before running the script, especially before the `create_baseline` step.
*   **Running:** Run the script, let it create the baseline. Then, manually change one of the files (e.g., add a line to `config.txt`), and create a new file (e.g., `new_file.txt`) in the `monitor_files` directory. Then press Enter in the terminal to continue the script and see the changes detected.
*   **Absolute vs. Relative Paths:** The script stores relative paths in the baseline, making it more portable if the root `monitor_files` directory is moved.
*   **Performance:** For very large directories with many files, creating a baseline or monitoring can take time. Consider optimizations if performance is critical.
