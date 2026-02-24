---
Title: Exercise 06: Platform Process Information with Python
Objective: Learn to retrieve and display information about running processes on the local system using Python's `subprocess` module to execute native operating system commands (`tasklist` on Windows, `ps` on Linux/macOS). This is useful for system monitoring and incident response.
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
    *   Create a new Python file named `process_info.py`.

2.  **Import Modules**
    *   Add `import subprocess` and `import platform` at the beginning of your script. `platform` is used to detect the operating system.

3.  **Determine OS and Command**
    *   Use `platform.system()` to check if the OS is 'Windows' or 'Linux'/'Darwin' (macOS).
    *   Set the appropriate command:
        *   Windows: `tasklist`
        *   Linux/macOS: `ps aux`

4.  **Execute the Command using `subprocess`**
    *   Use `subprocess.run()` to execute the command.
    *   Capture the output using `capture_output=True` and `text=True` (or `universal_newlines=True` in older Python versions) to get output as a string.
    *   Handle potential errors (`check=True` will raise an exception for non-zero exit codes).

5.  **Parse and Display Output**
    *   Print the raw output or parse it further if desired (e.g., using string splitting or regex, as shown in the previous exercise). For this exercise, printing the raw output is sufficient.

## Example `process_info.py`

```python
import subprocess
import platform
import sys

def get_process_info():
    """Executes OS-specific commands to get process information."""
    os_name = platform.system()
    command = []

    if os_name == "Windows":
        command = ["tasklist"]
        print("--- Windows Process List (tasklist) ---")
    elif os_name == "Linux" or os_name == "Darwin": # Darwin is macOS
        command = ["ps", "aux"]
        print("--- Linux/macOS Process List (ps aux) ---")
    else:
        print(f"Unsupported operating system: {os_name}")
        sys.exit(1)

    try:
        # Run the command
        # capture_output=True captures stdout and stderr
        # text=True decodes stdout/stderr as text using default encoding
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
    get_process_info()
```

## Expected Output (Windows)

```
--- Windows Process List (tasklist) ---
Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
System Idle Process              0 Services                   0          4 K
System                           4 Services                   0      2,996 K
smss.exe                       296 Services                   0        920 K
...
```
## Expected Output (Linux/macOS)

```
--- Linux/macOS Process List (ps aux) ---
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.1 108368  6960 ?        Ss   Feb20   0:02 /sbin/init
root           2  0.0  0.0      0     0 ?        S    Feb20   0:00 [kthreadd]
...
```
![Expected output](images/exercise-06-output-win.png) *(Example for Windows)*

## Reflection Questions

1.  What is the advantage of using `subprocess.run()` over `os.system()` for executing external commands in Python?
2.  How would you modify this script to filter processes by a specific name (e.g., to find all 'chrome.exe' processes)?
3.  What security implications arise from executing external system commands within a Python script?

## Next Steps

*   [Exercise 07: Platform Network Information with Python](../python/07-platform-network-info.md)
*   Further reading: [Python `subprocess` module documentation](https://docs.python.org/3/library/subprocess.html) and [Python `platform` module documentation](https://docs.python.org/3/library/platform.html)

## Hints / Troubleshooting

*   **Permissions:** `tasklist` and `ps aux` typically do not require administrative privileges for basic listing, but some information might be restricted without them.
*   **Command Not Found:** Ensure that the `tasklist` or `ps` commands are available in your system's PATH environment variable.
*   **Parsing Output:** For more structured analysis, you would typically parse the `result.stdout` string using string manipulation methods or regular expressions, similar to the log parsing exercise.
*   **External Libraries:** For more robust and OS-independent process management, the external `psutil` library is highly recommended.
