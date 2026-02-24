---
Title: Exercise 04: File Hash Calculator with Python
Objective: Write a Python script to calculate the SHA256 hash of a given file. This is a crucial skill for file integrity monitoring, verifying downloads, and identifying known malicious files (based on their hash).
Estimated Time: 15-30 minutes
Tools Needed:
*   Python 3
*   A text editor
Setup Instructions:
1.  Ensure Python 3 is installed.
2.  Have a sample file ready to hash (e.g., a text file, an image, or any other document). You can create a simple `test.txt` file with some content.
---

## Steps

1.  **Create the Script File**
    *   Create a new Python file named `hash_calculator.py`.

2.  **Import the `hashlib` Module**
    *   Add `import hashlib` at the beginning of your script.

3.  **Define the Hashing Function**
    *   Create a function that takes a file path as input.
    *   Inside the function:
        *   Initialize a SHA256 hash object: `hasher = hashlib.sha256()`
        *   Open the file in binary read mode (`'rb'`).
        *   Read the file in chunks (e.g., 4096 bytes at a time) to handle large files efficiently.
        *   Update the hasher with each chunk: `hasher.update(chunk)`.
        *   After reading the entire file, get the hexadecimal representation of the hash: `hasher.hexdigest()`.
        *   Return the hash.

4.  **Get File Path from User and Call Function**
    *   Prompt the user to enter the path to the file.
    *   Call your hashing function with the provided path.
    *   Print the resulting hash.

## Example `hash_calculator.py`

```python
import hashlib
import os
import sys

def calculate_sha256(filepath):
    """Calculates the SHA256 hash of a file."""
    hasher = hashlib.sha256()
    try:
        with open(filepath, 'rb') as file:
            # Read file in chunks to handle large files efficiently
            while True:
                chunk = file.read(4096)  # Read 4KB at a time
                if not chunk:
                    break
                hasher.update(chunk)
        return hasher.hexdigest()
    except FileNotFoundError:
        print(f"Error: File not found at '{filepath}'")
        return None
    except IOError as e:
        print(f"Error reading file '{filepath}': {e}")
        return None

if __name__ == "__main__":
    file_to_hash = input("Enter the path to the file you want to hash: ")

    if not os.path.exists(file_to_hash):
        print(f"File '{file_to_hash}' does not exist. Please check the path.")
        sys.exit(1)
    
    # Ensure it's a file and not a directory
    if os.path.isdir(file_to_hash):
        print(f"'{file_to_hash}' is a directory. Please provide a file path.")
        sys.exit(1)

    sha256_hash = calculate_sha256(file_to_hash)

    if sha256_hash:
        print(f"
File: {file_to_hash}")
        print(f"SHA256 Hash: {sha256_hash}")

```

## Expected Output

```
Enter the path to the file you want to hash: test.txt

File: test.txt
SHA256 Hash: 9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08
```
*(Note: The hash above is for a `test.txt` file containing the text "hello world".)*


## Reflection Questions

1.  Why is it important to read a file in chunks when calculating its hash, especially for large files?
2.  How can file hashing be used by incident responders or security analysts?
3.  What is the difference between SHA256 and MD5, and why is SHA256 generally preferred for security purposes?

## Next Steps

*   [Exercise 05: Log File Parser with Python (using `re`)](../python/05-log-file-parser-regex.md)
*   Further reading: [Python `hashlib` module documentation](https://docs.python.org/3/library/hashlib.html)

## Hints / Troubleshooting

*   **File Not Found Error:** Double-check the file path you enter. Use absolute paths if you're unsure of the current working directory.
*   **Other Hash Algorithms:** The `hashlib` module supports other algorithms like MD5, SHA1, SHA512. You can experiment by changing `hashlib.sha256()` to `hashlib.md5()`, etc.
*   **Consistency:** The hash of a file depends on its exact binary content. Even a single byte change will result in a completely different hash.
