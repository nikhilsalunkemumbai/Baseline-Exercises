# Resources for Baseline-bite-size Exercises

This directory contains various resources to support the `Baseline-bite-size` exercises. This primarily includes sample data files used in different exercises.

## Table of Contents

1.  [Sample Data](#1-sample-data)
2.  [External Resources (Links)](#2-external-resources-links)

---

## 1. Sample Data

The `sample_data/` subdirectory contains small, pre-generated data files that are directly used in several exercises. This ensures you have consistent data to work with without needing to generate it yourself.

### `resources/sample_data/`

*   **`malicious_ips.txt`**:
    *   **Description**: A plaintext file listing example malicious IP addresses.
    *   **Usage**: Used in Wireshark exercises (e.g., `exercises/wireshark/15-ioc-hunting.md`) for filtering traffic based on Indicators of Compromise (IOCs).
*   **`sample.log`**:
    *   **Description**: A generic log file, mimicking common server access logs. It includes various HTTP requests, status codes, and user agents.
    *   **Usage**: Utilized in Python exercises (e.g., `exercises/python/05-log-file-parser-regex.md`) for log parsing and in Elastic Stack exercises for ingestion and analysis.
*   **`users.csv`**:
    *   **Description**: A Comma Separated Values (CSV) file containing hypothetical user data (ID, username, email, role, last_login).
    *   **Usage**: Primarily for Python exercises (e.g., `exercises/python/10-read-write-csv.md`) and Logstash exercises (`exercises/elastic-stack/07-ingest-csv-logstash.md`) to demonstrate data manipulation and ingestion.
*   **`nginx_access.log`**:
    *   **Description**: A sample Nginx web server access log file, formatted like typical Nginx logs.
    *   **Usage**: Used in Elastic Stack exercises (e.g., `exercises/elastic-stack/12-ingest-web-logs-nginx.md`) for web log analysis.

### How to Use Sample Data

When an exercise requires a sample data file, it will typically instruct you to use a file from the `resources/sample_data/` directory. You can reference these files directly in your commands or scripts.

## 2. External Resources (Links)

This section would list links to external tools, datasets, or documentation that might be too large to include directly in the repository, but are valuable for specific exercises or for further learning. (Currently, this section is a placeholder.)
