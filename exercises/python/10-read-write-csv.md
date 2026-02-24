---
Title: Exercise 10: Read and Write CSV Files with Python
Objective: Learn to read data from and write data to Comma Separated Value (CSV) files using Python's built-in `csv` module. This is a common requirement for processing security logs, vulnerability reports, or managing data for incident response.
Estimated Time: 15-30 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Create a sample CSV file named `users.csv` in your working directory with the following content:
    ```csv
    username,email,role,last_login
    john.doe,john.doe@example.com,user,2026-02-23T10:00:00Z
    jane.smith,jane.smith@example.com,admin,2026-02-24T11:30:00Z
    bob.johnson,bob.johnson@example.com,user,2026-02-22T09:15:00Z
    ```
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `csv_processor.py`.

2.  **Import the `csv` Module**
    *   Add `import csv` at the beginning of your script.

3.  **Read from a CSV File**
    *   Open `users.csv` for reading (`'r'`).
    *   Create a `csv.reader` object.
    *   Iterate through the rows. The first row usually contains headers.
    *   You can also use `csv.DictReader` to read rows as dictionaries, where keys are column headers.

4.  **Write to a CSV File**
    *   Open a new file (e.g., `active_users.csv`) for writing (`'w'`) with `newline=''` (important to prevent extra blank rows).
    *   Create a `csv.writer` object.
    *   Write a header row using `writer.writerow()`.
    *   Write data rows using `writer.writerow()`.
    *   You can also use `csv.DictWriter` for writing dictionaries to CSV.

## Example `csv_processor.py`

```python
import csv
import sys

def process_users_csv(input_filepath, output_filepath):
    """Reads user data from input CSV and writes active users to output CSV."""
    users_data = []
    
    # --- Read from CSV ---
    print(f"--- Reading from '{input_filepath}' ---")
    try:
        with open(input_filepath, mode='r', newline='', encoding='utf-8') as infile:
            reader = csv.DictReader(infile)
            if reader.fieldnames:
                print(f"Headers: {', '.join(reader.fieldnames)}")
            
            for row in reader:
                users_data.append(row)
                print(f"Read: {row}")
    except FileNotFoundError:
        print(f"Error: Input file '{input_filepath}' not found.")
        sys.exit(1)
    except Exception as e:
        print(f"An error occurred while reading the CSV: {e}")
        sys.exit(1)

    # --- Process Data (Example: Filter for users who logged in recently or are admins) ---
    active_admins = []
    for user in users_data:
        # Example condition: an admin who logged in within the last 2 days (simplistic)
        # For actual date comparison, convert 'last_login' to datetime objects.
        if user.get('role') == 'admin' and user.get('last_login', '') >= '2026-02-22':
            active_admins.append(user)

    # --- Write to CSV ---
    print(f"
--- Writing active admins to '{output_filepath}' ---")
    if not active_admins:
        print("No active admins found to write.")
        return # Exit if no data to write

    try:
        with open(output_filepath, mode='w', newline='', encoding='utf-8') as outfile:
            # Get fieldnames from the first dictionary
            fieldnames = active_admins[0].keys()
            writer = csv.DictWriter(outfile, fieldnames=fieldnames)

            writer.writeheader() # Write the header row
            for row in active_admins:
                writer.writerow(row)
        print(f"Successfully wrote {len(active_admins)} records to '{output_filepath}'.")
    except Exception as e:
        print(f"An error occurred while writing the CSV: {e}")
        sys.exit(1)


if __name__ == "__main__":
    input_csv = "users.csv"
    output_csv = "active_admins.csv"
    process_users_csv(input_csv, output_csv)

```

## Expected Output (Terminal)

```
--- Reading from 'users.csv' ---
Headers: username, email, role, last_login
Read: {'username': 'john.doe', 'email': 'john.doe@example.com', 'role': 'user', 'last_login': '2026-02-23T10:00:00Z'}
Read: {'username': 'jane.smith', 'email': 'jane.smith@example.com', 'role': 'admin', 'last_login': '2026-02-24T11:30:00Z'}
Read: {'username': 'bob.johnson', 'email': 'bob.johnson@example.com', 'role': 'user', 'last_login': '2026-02-22T09:15:00Z'}

--- Writing active admins to 'active_admins.csv' ---
Successfully wrote 1 records to 'active_admins.csv'.
```
*(After running, check the `active_admins.csv` file in your directory. Its content should be:)*
```csv
username,email,role,last_login
jane.smith,jane.smith@example.com,admin,2026-02-24T11:30:00Z
```


## Reflection Questions

1.  What is the significance of `newline=''` when opening CSV files for writing, and what happens if it's omitted?
2.  Compare `csv.reader`/`csv.writer` with `csv.DictReader`/`csv.DictWriter`. When would you choose one over the other?
3.  How can Python's CSV processing capabilities be used to automate tasks in security operations, such as analyzing vulnerability scan results or user access logs?

## Next Steps

*   [Exercise 11: OS Command Execution with Python](../python/11-os-command-execution.md)
*   Further reading: [Python `csv` module documentation](https://docs.python.org/3/library/csv.html)

## Hints / Troubleshooting

*   **`FileNotFoundError`:** Ensure `users.csv` is in the same directory as your script or provide the full path.
*   **Encoding Issues:** If you encounter `UnicodeDecodeError`, try specifying a different `encoding` (e.g., `'latin-1'`) when opening the file, although `utf-8` is a good default.
*   **Delimiter:** If your CSV uses a different delimiter (e.g., tab-separated values), you can specify it using the `delimiter` argument in `csv.reader` or `csv.writer`.
*   **Large Files:** The `csv` module is efficient for large files as it processes them row by row, not loading the entire file into memory at once.
