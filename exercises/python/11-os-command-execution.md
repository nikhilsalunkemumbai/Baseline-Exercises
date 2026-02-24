---
Title: Exercise 11: OS Command Execution with Python
Objective: Learn to safely execute external operating system commands from within Python scripts using the `subprocess` module. This is essential for integrating Python automation with existing shell tools, administering systems, or orchestrating complex tasks.
Estimated Time: 15-30 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Open a terminal or command prompt.
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `run_os_command.py`.

2.  **Import `subprocess`**
    *   Add `import subprocess` at the beginning of your script.

3.  **Execute a Simple Command (`ls` or `dir`)**
    *   Use `subprocess.run()` with `capture_output=True` and `text=True` to get the command's output.
    *   Print the standard output and standard error.
    *   Check the `returncode` attribute to see if the command was successful (0 typically means success).

4.  **Handle Command Arguments**
    *   Pass command arguments as a list of strings to `subprocess.run()`. This is safer than constructing a single string command, as it avoids shell injection vulnerabilities.
    *   Example: `['ls', '-l', '/tmp']` or `['dir', 'C:\Windows']`.

5.  **Error Handling**
    *   Use a `try-except` block to catch `subprocess.CalledProcessError` if `check=True` is used, which indicates the command exited with a non-zero status.

## Example `run_os_command.py`

```python
import subprocess
import platform
import sys

def run_command(command_parts):
    """
    Executes an OS command and prints its output.
    command_parts: A list of strings representing the command and its arguments.
    """
    try:
        print(f"
--- Executing command: {' '.join(command_parts)} ---")
        # subprocess.run is recommended for most use cases.
        # capture_output=True collects stdout and stderr.
        # text=True decodes stdout/stderr as text.
        # check=True raises a CalledProcessError if the command returns a non-zero exit code.
        result = subprocess.run(command_parts, capture_output=True, text=True, check=True)
        
        print("
--- STDOUT ---")
        print(result.stdout)
        
        if result.stderr:
            print("
--- STDERR ---")
            print(result.stderr)
        
        print(f"Command exited with code: {result.returncode}")
        return result.stdout
    
    except FileNotFoundError:
        print(f"Error: Command '{command_parts[0]}' not found. Is it in your system's PATH?")
        return None
    except subprocess.CalledProcessError as e:
        print(f"Error: Command '{' '.join(command_parts)}' failed with exit code {e.returncode}")
        print(f"Error output (STDERR): {e.stderr}")
        return None
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
        return None

if __name__ == "__main__":
    if platform.system() == "Windows":
        # Example for Windows: List current directory contents
        run_command(["dir"])
        # Example for Windows: Get network configuration
        run_command(["ipconfig", "/all"])
        # Example for Windows: A command that will likely fail (bad command)
        run_command(["thisisnotacommand"])
    else: # Linux or macOS
        # Example for Linux/macOS: List current directory contents
        run_command(["ls", "-l"])
        # Example for Linux/macOS: Get network configuration
        run_command(["ip", "a"])
        # Example for Linux/macOS: A command that will likely fail (bad command)
        run_command(["thisisnotacommand"])
```

## Expected Output (Example on Linux for `ls -l` and `ip a`)

```
--- Executing command: ls -l ---

--- STDOUT ---
total 28
-rw-r--r-- 1 user user 1569 Feb 24 10:00 access.log
-rw-r--r-- 1 user user 2048 Feb 24 10:05 csv_processor.py
-rw-r--r-- 1 user user 1200 Feb 24 09:45 hash_calculator.py
...
Command exited with code: 0

--- Executing command: ip a ---

--- STDOUT ---
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
...
Command exited with code: 0

--- Executing command: thisisnotacommand ---
Error: Command 'thisisnotacommand' not found. Is it in your system's PATH?
```


## Reflection Questions

1.  Why is it generally recommended to pass command arguments as a list of strings (e.g., `['ls', '-l']`) to `subprocess.run()` instead of a single string (e.g., `'ls -l'`)?
2.  In what scenarios would `subprocess.run(..., shell=True)` be useful, and what are the security risks associated with it?
3.  How can Python scripts that execute OS commands be used in automated security tasks like system hardening, log collection, or malware analysis?

## Next Steps

*   [Exercise 12: Environment Variable Manipulation with Python](../python/12-env-var-manipulation.md)
*   Further reading: [Python `subprocess` module documentation](https://docs.python.org/3/library/subprocess.html)

## Hints / Troubleshooting

*   **`FileNotFoundError`:** This usually means the command itself (e.g., `ls`, `dir`) is not found in your system's PATH.
*   **`CalledProcessError`:** This means the command executed, but it returned a non-zero exit code (indicating an error or non-success state for that command). The `stderr` attribute of the exception will often contain the error message from the command.
*   **Permissions:** Some commands require administrative or root privileges. If a command fails, try running your Python script with `sudo python script.py` (Linux/macOS) or as Administrator (Windows).
*   **Blocking vs. Non-Blocking:** `subprocess.run()` is a blocking call; the Python script will wait until the command completes. For non-blocking execution, consider `subprocess.Popen`.
