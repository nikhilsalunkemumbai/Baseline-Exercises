# Troubleshooting Guide for Baseline-Exercises

This guide provides solutions to common issues you might encounter while setting up your environment or working through the exercises. Always refer back to the [`SETUP.md`](SETUP.md) for correct installation steps and configurations.

## Table of Contents

1.  [General Environment Issues](#1-general-environment-issues)
2.  [Wireshark Troubleshooting](#2-wireshark-troubleshooting)
3.  [Sysinternals Suite Troubleshooting](#3-sysinternals-suite-troubleshooting)
4.  [Nmap Troubleshooting](#4-nmap-troubleshooting)
5.  [Python Troubleshooting](#5-python-troubleshooting)
6.  [Elastic Stack Troubleshooting](#6-elastic-stack-troubleshooting)
7.  [Sample Data Issues](#7-sample-data-issues)

---

## 1. General Environment Issues

*   **"Command Not Found" or "Tool Not Recognized"**:
    *   **Cause**: The tool's executable directory is not in your system's `PATH` environment variable, or the tool is not installed.
    *   **Solution**:
        *   Verify the tool is installed correctly.
        *   Add the tool's `bin` (or executable) directory to your system's `PATH`. Remember to restart your terminal/command prompt after modifying `PATH`.
        *   On Windows, ensure you selected "Add Python to PATH" during Python installation.

*   **Permissions Errors (Access Denied)**:
    *   **Cause**: You are trying to perform an action (e.g., install software, capture network traffic, write to system directories) without sufficient privileges.
    *   **Solution**:
        *   Run your terminal/command prompt as an Administrator (Windows) or use `sudo` (Linux/macOS) for commands that require elevated privileges.
        *   Check file/folder permissions if you're having trouble reading/writing specific files.

*   **Port Conflicts**:
    *   **Cause**: Another application is already using a port required by one of your tools (e.g., Elasticsearch defaults to 9200, Kibana to 5601).
    *   **Solution**:
        *   Identify the process using the port (e.g., `netstat -ano | findstr :<port_number>` on Windows, `lsof -i :<port_number>` on Linux/macOS).
        *   Stop the conflicting application or configure your tool to use a different port in its configuration file.

---

## 2. Wireshark Troubleshooting

*   **No Interfaces Listed / Cannot Capture Traffic**:
    *   **Cause**: Npcap (Windows) or libpcap (Linux/macOS) is not installed or not working correctly, or you lack the necessary permissions.
    *   **Solution**:
        *   Ensure Npcap (Windows) was installed with Wireshark. Reinstall Wireshark if necessary and make sure Npcap is checked.
        *   On Linux, ensure your user is part of the `wireshark` group (`sudo usermod -aG wireshark $USER`) and log out/in.
        *   Run Wireshark as Administrator/root (though this is generally discouraged for daily use due to security).

*   **"No packets captured" / Capture Filter Not Working**:
    *   **Cause**: The capture filter is too restrictive, or you're capturing on the wrong interface.
    *   **Solution**:
        *   Double-check your chosen interface.
        *   Temporarily remove the capture filter to see if any traffic is captured. If it is, refine your filter syntax.
        *   Ensure the target traffic is actually flowing through the interface you are monitoring.

---

## 3. Sysinternals Suite Troubleshooting

*   **"This app can't run on your PC" / Tool Not Launching**:
    *   **Cause**: Sysinternals tools are Windows-only. You might be trying to run them on another OS, or there's a corruption in the download/extraction.
    *   **Solution**:
        *   Confirm you are on a Windows operating system.
        *   Re-download and re-extract the Sysinternals Suite.

*   **"Access is denied" when running a tool**:
    *   **Cause**: The tool requires Administrator privileges to perform its function.
    *   **Solution**:
        *   Run your command prompt or PowerShell session as an Administrator before executing Sysinternals tools.

*   **Tool not found even after adding to PATH**:
    *   **Cause**: The terminal session was opened before the PATH variable was updated.
    *   **Solution**:
        *   Close and reopen your command prompt or PowerShell session after modifying the system's `PATH`.

---

## 4. Nmap Troubleshooting

*   **"Host seems down" or No Response**:
    *   **Cause**: Target host is actually offline, a firewall is blocking Nmap's probes, or your local firewall is blocking outgoing connections.
    *   **Solution**:
        *   Ping the target host first to confirm it's reachable (`ping <IP_address>`).
        *   Temporarily disable your operating system's firewall (e.g., Windows Defender Firewall) to rule it out.
        *   Use Nmap's `-Pn` (no ping) option to force a port scan even if the host doesn't respond to ping.
        *   Try different Nmap options (e.g., `-sS` for SYN scan, `-sT` for TCP connect scan) to bypass basic firewalls.

*   **Slow Scans**:
    *   **Cause**: Nmap's default timing might be too conservative, or network latency is high.
    *   **Solution**:
        *   Experiment with timing templates (e.g., `-T4` or `-T5` for faster scans, but be aware of network congestion or target detection).
        *   Scan smaller port ranges or fewer hosts at a time.

---

## 5. Python Troubleshooting

*   **"ModuleNotFoundError: No module named '<module_name>'"**:
    *   **Cause**: The required Python package is not installed, or you are running the script outside of a virtual environment where it *is* installed.
    *   **Solution**:
        *   Activate your virtual environment (`.venv\Scripts\activate` or `source .venv/bin/activate`).
        *   Install the missing package using `pip install <package_name>`.

*   **Python Scripts Not Executing / Syntax Errors**:
    *   **Cause**: Incorrect Python version, syntax issues, or incorrect execution method.
    *   **Solution**:
        *   Ensure you are using `python` or `python3` correctly (e.g., `python your_script.py`).
        *   Carefully review the script for typos, indentation errors, or other syntax issues.
        *   Check the shebang line (`#!/usr/bin/env python3`) if running on Linux/macOS.

---

## 6. Elastic Stack Troubleshooting

*   **Elasticsearch/Kibana Not Starting (Stuck or Exiting Immediately)**:
    *   **Cause**: Incorrect `JAVA_HOME` setup, YAML configuration errors, or insufficient memory.
    *   **Solution**:
        *   Check the logs for detailed error messages (e.g., `elasticsearch/logs/`, `kibana/logs/`).
        *   Verify `JAVA_HOME` is set correctly and points to a compatible JDK.
        *   Double-check `elasticsearch.yml` and `kibana.yml` for syntax errors (YAML is strict about indentation).
        *   Ensure your system has enough RAM (minimum 8GB for the stack).

*   **Kibana Cannot Connect to Elasticsearch**:
    *   **Cause**: Elasticsearch is not running, or `elasticsearch.hosts` in `kibana.yml` is incorrect.
    *   **Solution**:
        *   Verify Elasticsearch is running and accessible at `http://localhost:9200`.
        *   Ensure `elasticsearch.hosts` in `kibana.yml` is set to `["http://localhost:9200"]`.

*   **Filebeat/Logstash Not Ingesting Data**:
    *   **Cause**: Incorrect configuration file paths, permissions to read log files, or incorrect output settings.
    *   **Solution**:
        *   Check Filebeat/Logstash logs for errors.
        *   Ensure `filebeat.inputs` (for Filebeat) or `input` section (for Logstash) correctly points to your sample log files.
        *   Verify the user running Filebeat/Logstash has read permissions on the log files.
        *   Ensure `output.elasticsearch` or `output.logstash` settings are correct and the target (Elasticsearch/Logstash) is running.

---

## 7. Sample Data Issues

*   **"File not found" when referencing sample data**:
    *   **Cause**: Incorrect path to the sample data file or the file was not created/downloaded.
    *   **Solution**:
        *   Double-check the path in your command or script (e.g., `resources/sample_data/malicious_ips.txt`).
        *   Ensure the `resources/sample_data/` directory and its contents exist. Refer to the [`SETUP.md`](SETUP.md) for instructions on creating/obtaining sample data.
