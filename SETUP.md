# Setup Guide for Baseline-Exercises

This guide provides step-by-step instructions to set up your environment for all exercises in the `Baseline-Exercises` repository. It covers the installation of necessary tools, preparation of sample data, and configuration tips.

## Table of Contents

1.  [General Prerequisites](#1-general-prerequisites)
2.  [Wireshark Setup](#2-wireshark-setup)
3.  [Sysinternals Suite Setup](#3-sysinternals-suite-setup)
4.  [Nmap Setup](#4-nmap-setup)
5.  [Python Setup](#5-python-setup)
6.  [Elastic Stack Setup (Elasticsearch, Kibana, Filebeat, Logstash)](#6-elastic-stack-setup-elasticsearch-kibana-filebeat-logstash)
7.  [Sample Data and Configuration Files](#7-sample-data-and-configuration-files)

---

## 1. General Prerequisites

Before installing the tools, ensure your system meets these general requirements:

*   **Operating System**: Windows 10/11 (for Sysinternals), Linux (Ubuntu/Debian, CentOS/RHEL) or macOS. Exercises are designed to be largely cross-platform where applicable, but Sysinternals is Windows-specific.
*   **Administrative Privileges**: Many installations and network configurations will require administrator/root access.
*   **Internet Connection**: For downloading tools and packages.
*   **Disk Space**: At least 20-30 GB free for installations, sample data, and Elastic Stack data.
*   **RAM**: Minimum 8 GB recommended, especially for running Elastic Stack components.

---

## 2. Wireshark Setup

Wireshark is a free and open-source packet analyzer.

1.  **Download**:
    *   Visit the official Wireshark website: [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)
    *   Download the installer appropriate for your operating system (Windows Installer, macOS .dmg, or Linux packages).
2.  **Installation**:
    *   **Windows/macOS**: Run the downloaded installer. Follow the on-screen prompts. Ensure you install **Npcap** (or WinPcap on older systems) when prompted, as it's required for live packet capture.
    *   **Linux (Debian/Ubuntu)**:
        ```bash
        sudo apt update
        sudo apt install wireshark
        # Configure Wireshark to allow non-root users to capture packets (recommended)
        sudo dpkg-reconfigure wireshark-common
        sudo usermod -aG wireshark $USER
        ```
        (You may need to log out and log back in for changes to take effect.)
3.  **Verification**: Open Wireshark. You should see a list of network interfaces available for capture.

---

## 3. Sysinternals Suite Setup

The Sysinternals Suite is a collection of advanced system utilities for Windows.

1.  **Download**:
    *   Visit the official Sysinternals Suite download page: [https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
    *   Download the `SysinternalsSuite.zip` file.
2.  **Extraction**:
    *   Create a dedicated folder for Sysinternals, e.g., `C:\Sysinternals`.
    *   Extract the contents of `SysinternalsSuite.zip` into this folder.
3.  **Environment Path (Optional but Recommended)**:
    *   Add the `C:\Sysinternals` directory to your system's `PATH` environment variable. This allows you to run Sysinternals tools from any command prompt.
    *   **Windows 10/11**:
        *   Search for "Environment Variables" and open "Edit the system environment variables".
        *   Click "Environment Variables..."
        *   Under "System variables", select "Path" and click "Edit...".
        *   Click "New" and add `C:\Sysinternals`. Click "OK" on all windows.
        *   Restart your command prompt/PowerShell for the changes to take effect.
4.  **Verification**: Open a command prompt and type `procexp.exe`. Process Explorer should launch.

---

## 4. Nmap Setup

Nmap ("Network Mapper") is a free and open-source utility for network discovery and security auditing.

1.  **Download**:
    *   Visit the official Nmap download page: [https://nmap.org/download.html](https://nmap.org/download.html)
    *   Download the installer appropriate for your operating system (Windows installer, macOS .dmg, or Linux packages).
2.  **Installation**:
    *   **Windows**: Run the installer. Ensure you install **Npcap** (if not already installed by Wireshark) when prompted.
    *   **macOS**: Open the .dmg and drag Nmap to your Applications folder.
    *   **Linux (Debian/Ubuntu)**:
        ```bash
        sudo apt update
        sudo apt install nmap
        ```
3.  **Verification**: Open a terminal/command prompt and type `nmap --version`. You should see the Nmap version information.

---

## 5. Python Setup

Python is a versatile programming language crucial for automation and scripting.

1.  **Download**:
    *   Visit the official Python website: [https://www.python.org/downloads/](https://www.python.org/downloads/)
    *   Download the latest stable version of Python 3 (e.g., Python 3.9+ recommended).
2.  **Installation**:
    *   **Windows**: Run the installer. **IMPORTANT**: Check the box that says "Add Python X.Y to PATH" during installation. This makes Python and pip accessible from the command line.
    *   **macOS**: Python 3 is often pre-installed or can be installed via Homebrew (`brew install python`).
    *   **Linux**: Python 3 is typically pre-installed or can be installed via your distribution's package manager (`sudo apt install python3`).
3.  **Verification**: Open a terminal/command prompt and type `python --version` (or `python3 --version` on some systems) and `pip --version` (or `pip3 --version`). You should see their version numbers.
4.  **Virtual Environments (Recommended)**: For Python projects, it's good practice to use virtual environments to manage dependencies.
    ```bash
    # Create a virtual environment in your project directory
    python -m venv .venv
    # Activate the virtual environment
    # On Windows:
    .venv\Scripts\activate
    # On Linux/macOS:
    source .venv/bin/activate
    ```
    Install exercise-specific dependencies using `pip install <package-name>` while the virtual environment is active.

---

## 6. Elastic Stack Setup (Elasticsearch, Kibana, Filebeat, Logstash)

The Elastic Stack (ELK) is used for searching, analyzing, and visualizing log data. For these exercises, we'll focus on a single-node setup.

**Important**: Elastic Stack components require Java.

1.  **Java Development Kit (JDK) Installation**:
    *   Download and install a compatible JDK (e.g., OpenJDK 11 or 17). Adoptium Temurin is a good choice: [https://adoptium.net/temurin/releases/](https://adoptium.net/temurin/releases/)
    *   Ensure the `JAVA_HOME` environment variable is set and added to your system `PATH`.
    *   Verify by running `java -version` in a new terminal.

2.  **Elasticsearch Installation**:
    *   **Download**: [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)
    *   **Installation**:
        *   **Windows**: Download the `.zip` file, extract to `C:\elastic\elasticsearch-X.Y.Z`.
        *   **Linux**: Download the `.tar.gz` file, extract to `/opt/elastic/elasticsearch-X.Y.Z`.
    *   **Configuration**:
        *   Navigate to the `config` directory (e.g., `C:\elastic\elasticsearch-X.Y.Z\config`).
        *   Edit `elasticsearch.yml`. For a single-node setup, uncomment and set:
            ```yaml
            network.host: localhost
            http.port: 9200
            discovery.type: single-node
            ```
    *   **Run Elasticsearch**:
        *   Open a terminal/command prompt.
        *   Navigate to the `bin` directory (e.g., `C:\elastic\elasticsearch-X.Y.Z\bin`).
        *   Run `./elasticsearch` (Linux/macOS) or `elasticsearch.bat` (Windows).
        *   Wait for it to start (may take a minute or two).
        *   Verify by opening [http://localhost:9200](http://localhost:9200) in your browser. You should see a JSON response.

3.  **Kibana Installation**:
    *   **Download**: [https://www.elastic.co/downloads/kibana](https://www.elastic.co/downloads/kibana)
    *   **Installation**:
        *   **Windows**: Download the `.zip` file, extract to `C:\elastic\kibana-X.Y.Z`.
        *   **Linux**: Download the `.tar.gz` file, extract to `/opt/elastic/kibana-X.Y.Z`.
    *   **Configuration**:
        *   Navigate to the `config` directory (e.g., `C:\elastic\kibana-X.Y.Z\config`).
        *   Edit `kibana.yml`. Uncomment and set:
            ```yaml
            server.port: 5601
            server.host: "localhost"
            elasticsearch.hosts: ["http://localhost:9200"]
            ```
    *   **Run Kibana**:
        *   Open a *new* terminal/command prompt (keep Elasticsearch running in its own terminal).
        *   Navigate to the `bin` directory (e.g., `C:\elastic\kibana-X.Y.Z\bin`).
        *   Run `./kibana` (Linux/macOS) or `kibana.bat` (Windows).
        *   Wait for it to start.
        *   Verify by opening [http://localhost:5601](http://localhost:5601) in your browser. You should see the Kibana login page (if security is enabled) or dashboard.

4.  **Filebeat Installation**:
    *   **Download**: [https://www.elastic.co/downloads/beats/filebeat](https://www.elastic.co/downloads/beats/filebeat)
    *   **Installation**:
        *   **Windows**: Download the `.zip` file, extract to `C:\elastic\filebeat-X.Y.Z`.
        *   **Linux**: Download the `.tar.gz` file, extract to `/opt/elastic/filebeat-X.Y.Z`.
    *   **Configuration**:
        *   Navigate to the Filebeat directory.
        *   Edit `filebeat.yml`.
            *   Uncomment and configure the `output.elasticsearch` section (or `output.logstash` if you're using Logstash).
            *   For basic setup directly to Elasticsearch:
                ```yaml
                output.elasticsearch:
                  hosts: ["localhost:9200"]
                ```
            *   Enable relevant modules (e.g., `system`, `nginx`) if you plan to use them, or define custom `filebeat.inputs` sections to read specific log files.
    *   **Run Filebeat**:
        *   Open a *new* terminal/command prompt.
        *   Navigate to the Filebeat directory.
        *   Run `./filebeat -e` (Linux/macOS) or `filebeat.exe -e` (Windows).

5.  **Logstash Installation (Optional, for advanced ingestion/enrichment)**:
    *   **Download**: [https://www.elastic.co/downloads/logstash](https://www.elastic.co/downloads/logstash)
    *   **Installation**:
        *   **Windows**: Download the `.zip` file, extract to `C:\elastic\logstash-X.Y.Z`.
        *   **Linux**: Download the `.tar.gz` file, extract to `/opt/elastic/logstash-X.Y.Z`.
    *   **Configuration**:
        *   Navigate to the `config` directory.
        *   Create a Logstash pipeline configuration file (e.g., `my_pipeline.conf`). A basic example:
            ```conf
            input {
              beats {
                port => 5044
              }
            }
            output {
              elasticsearch {
                hosts => ["localhost:9200"]
                index => "logstash-custom-%{+YYYY.MM.dd}"
              }
              stdout { codec => rubydebug } # For debugging
            }
            ```
            If using Logstash, configure Filebeat's `output.logstash` to point here instead of `output.elasticsearch`.
    *   **Run Logstash**:
        *   Open a *new* terminal/command prompt.
        *   Navigate to the Logstash directory.
        *   Run `./bin/logstash -f config/my_pipeline.conf` (Linux/macOS) or `.\bin\logstash.bat -f config\my_pipeline.conf` (Windows).

---

## 7. Sample Data and Configuration Files

Many exercises rely on sample data or specific configuration files. These are typically located in the `resources/sample_data/` directory.

### Location:
*   `resources/sample_data/`

### Important Sample Files:

*   **`malicious_ips.txt`**: A simple text file containing a list of IP addresses, one per line. Used in Wireshark exercises for IOC hunting.
    *   *Example Content:*
        ```
        192.168.1.100
        10.0.0.50
        172.16.0.25
        ```
*   **`sample.log`**: A generic log file (e.g., Apache access logs, system logs). Used in Python and Elastic Stack exercises.
    *   *Example Content (can be generated or found online):*
        ```
        192.168.1.1 - [24/Feb/2026:10:00:00 +0000] "GET /index.html HTTP/1.1" 200 1234 "-" "Mozilla/5.0"
        192.168.1.2 - [24/Feb/2026:10:00:05 +0000] "POST /login HTTP/1.1" 401 500 "http://example.com/login" "User-Agent: AttackerScript/1.0"
        10.0.0.10 - [24/Feb/2026:10:00:10 +0000] "GET /api/data HTTP/1.1" 200 2000 "-" "Python-urllib/3.8"
        ```
*   **`users.csv`**: A Comma Separated Values file containing user data. Used in Python and Logstash exercises.
    *   *Example Content:*
        ```csv
        id,username,email,role
        1,john_doe,john.doe@example.com,user
        2,jane_smith,jane.smith@example.com,admin
        3,bob_johnson,bob.johnson@example.com,user
        ```
*   **`nginx_access.log`**: A standard Nginx access log file. Used in Elastic Stack exercises.
    *   *Example Content (similar to Apache logs, can be generated or found online):*
        ```
        192.168.1.10 - - [24/Feb/2026:10:00:00 +0000] "GET /index.html HTTP/1.1" 200 1560 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.127 Safari/537.36"
        10.0.0.20 - - [24/Feb/2026:10:00:15 +0000] "POST /api/v1/auth HTTP/1.1" 403 240 "-" "curl/7.64.1"
        ```

### Generating Sample Data

If the `resources/sample_data/` directory does not contain these files, you can create them manually or use simple scripts (e.g., Python) to generate realistic-looking data. The exercises will provide context on what kind of data is expected.

This `SETUP.md` document aims to get you up and running with the necessary environment quickly. Refer to individual exercise files for specific configurations or dependencies.
