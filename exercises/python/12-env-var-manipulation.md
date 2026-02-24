---
Title: Exercise 12: Environment Variable Manipulation with Python
Objective: Learn to read and set environment variables using Python's built-in `os` module. Environment variables are crucial for configuring applications, storing system-wide settings, and passing sensitive information (like API keys) securely without hardcoding them into scripts.
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
    *   Create a new Python file named `env_vars.py`.

2.  **Import the `os` Module**
    *   Add `import os` at the beginning of your script.

3.  **Read an Existing Environment Variable**
    *   Use `os.getenv('VARIABLE_NAME')` or `os.environ.get('VARIABLE_NAME')` to read the value of an environment variable.
    *   Example: Read `PATH` or `HOME`/`USERPROFILE`.
    *   `os.environ` acts like a dictionary, so you can also directly access `os.environ['VARIABLE_NAME']`, but `get()` is safer as it won't raise a `KeyError` if the variable doesn't exist.

4.  **Set a New Environment Variable**
    *   Use `os.environ['NEW_VARIABLE'] = 'value'` to set a new environment variable.
    *   *Note:* Changes made with `os.environ` are usually local to the current Python process and its child processes. They typically do not persist after the script exits or affect the parent shell.

5.  **Unset an Environment Variable**
    *   Use `del os.environ['VARIABLE_NAME']` to remove an environment variable.

6.  **List All Environment Variables**
    *   Iterate through `os.environ` to see all current environment variables.

## Example `env_vars.py`

```python
import os

def manipulate_env_vars():
    """Demonstrates reading, setting, and unsetting environment variables."""

    print("--- Reading Environment Variables ---")
    
    # Get a common environment variable (e.g., PATH)
    path_env = os.getenv('PATH') # Use getenv for safer access (returns None if not found)
    print(f"PATH: {path_env[:100]}..." if path_env else "PATH not set") # Print first 100 chars

    # Get HOME or USERPROFILE depending on OS
    home_dir = os.getenv('HOME') or os.getenv('USERPROFILE')
    print(f"HOME/USERPROFILE: {home_dir}" if home_dir else "HOME/USERPROFILE not set")

    # Try to get a variable that might not exist
    non_existent_var = os.getenv('NON_EXISTENT_VAR')
    print(f"NON_EXISTENT_VAR: {non_existent_var}")

    print("
--- Setting a New Environment Variable ---")
    # Set a new environment variable
    os.environ['MY_TEST_VARIABLE'] = 'This is a secret message for this script.'
    print(f"MY_TEST_VARIABLE set to: {os.environ.get('MY_TEST_VARIABLE')}")

    print("
--- Listing All Environment Variables (Snippet) ---")
    # Print a few to demonstrate
    count = 0
    for key, value in os.environ.items():
        print(f"{key}={value[:50]}...") # Print first 50 chars of value
        count += 1
        if count >= 5: # Limit to first 5 for brevity
            break

    print("
--- Unsetting an Environment Variable ---")
    # Unset the variable
    if 'MY_TEST_VARIABLE' in os.environ:
        del os.environ['MY_TEST_VARIABLE']
        print("MY_TEST_VARIABLE unset.")
    print(f"MY_TEST_VARIABLE after unsetting: {os.environ.get('MY_TEST_VARIABLE')}")

    # Demonstrate changes are local to this process
    print("
--- Verifying Variable Persistence (Check after script exits) ---")
    print("Run 'echo %MY_TEST_VARIABLE%' (Windows) or 'echo $MY_TEST_VARIABLE' (Linux/macOS) in your shell AFTER this script finishes.")
    print("You should find that MY_TEST_VARIABLE is NOT set in your shell.")


if __name__ == "__main__":
    manipulate_env_vars()
```

## Expected Output

```
--- Reading Environment Variables ---
PATH: C:\Python39\Scripts\;C:\Python39\;C:\WINDOWS\system32;C:\WINDOWS;C:\WINDOWS\System32\Wbem;C:\W...
HOME/USERPROFILE: C:\Users\YourUser
NON_EXISTENT_VAR: None

--- Setting a New Environment Variable ---
MY_TEST_VARIABLE set to: This is a secret message for this script.

--- Listing All Environment Variables (Snippet) ---
ALLUSERSPROFILE=C:\ProgramData...
APPDATA=C:\Users\YourUser\AppData\Roaming...
COMMONPROGRAMFILES=C:\Program Files\Common Files...
COMPUTERNAME=MYCOMPUTER...
COMSPEC=C:\WINDOWS\system32\cmd.exe...
... (continues for other variables)

--- Unsetting an Environment Variable ---
MY_TEST_VARIABLE unset.
MY_TEST_VARIABLE after unsetting: None

--- Verifying Variable Persistence (Check after script exits) ---
Run 'echo %MY_TEST_VARIABLE%' (Windows) or 'echo $MY_TEST_VARIABLE' (Linux/macOS) in your shell AFTER this script finishes.
You should find that MY_TEST_VARIABLE is NOT set in your shell.
```


## Reflection Questions

1.  Why is it generally better to store sensitive information (like API keys) in environment variables rather than hardcoding them into scripts?
2.  Explain why changes made to environment variables within a Python script typically do not persist in the parent shell after the script exits.
3.  How can environment variables be utilized for configuring different environments (e.g., development, staging, production) for a security tool?

## Next Steps

*   [Exercise 13: File Integrity Monitor with Python](../python/13-file-integrity-monitor.md)
*   Further reading: [Python `os` module documentation](https://docs.python.org/3/library/os.html)

## Hints / Troubleshooting

*   **Persistence:** If you need an environment variable to persist for the current shell session, you typically set it directly in the shell (e.g., `export MY_VAR=value` on Linux/macOS, `set MY_VAR=value` on Windows Command Prompt, `$env:MY_VAR='value'` on PowerShell).
*   **Case Sensitivity:** Environment variable names are typically case-sensitive on Linux/macOS and case-insensitive on Windows, but it's best practice to treat them as case-sensitive for cross-platform compatibility.
*   **Security:** Be cautious about what information you store in environment variables, especially if other users or processes have access to your system's process environment.
