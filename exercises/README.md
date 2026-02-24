# Bite-Sized Threat Hunting & Incident Response Exercises

Welcome to the `exercises` directory! This is the core of the `Baseline-bite-size` repository, offering 96 hands-on challenges across 6 essential Threat Hunting & Incident Response (THIR) tools. Each exercise is designed to be completed in 1-2 hours, providing practical skills and tangible outputs for your portfolio.

## How to Use These Exercises

1.  **Setup Your Environment**: Before diving in, ensure your system is set up correctly by following the detailed instructions in [`SETUP.md`](../SETUP.md).
2.  **Choose Your Path**: You can work through the exercises sequentially for each tool, or jump to specific topics that interest you.
3.  **Execute and Document**: Follow the steps in each exercise, produce the expected outputs, and consider adding your findings to a personal portfolio.
4.  **Troubleshooting**: If you encounter issues, refer to the [`TROUBLESHOOTING.md`](../TROUBLESHOOTING.md) guide.

## Exercise Categories

Here you will find 16 exercises for each of the following tools:

*   [Wireshark](#wireshark) - Network Protocol Analyzer
*   [Sysinternals Suite](#sysinternals-suite) - Windows System Utilities
*   [Nmap](#nmap) - Network Scanner
*   [Python](#python) - Scripting for Automation
*   [Elastic Stack (ELK)](#elastic-stack-elk) - Log Management & Analysis
*   [NIST SP 800-61](#nist-sp-800-61) - Incident Response Framework

---

## Wireshark

A powerful network protocol analyzer that allows you to capture and interactively browse the data traveling on a computer network. Essential for understanding network communications and identifying anomalies.

*   [01-capture-http-request.md](wireshark/01-capture-http-request.md)
*   [02-filter-by-ip.md](wireshark/02-filter-by-ip.md)
*   [03-reconstruct-ftp-file.md](wireshark/03-reconstruct-ftp-file.md)
*   [04-detect-port-scan.md](wireshark/04-detect-port-scan.md)
*   [05-extract-dns-queries.md](wireshark/05-extract-dns-queries.md)
*   [06-follow-tcp-stream.md](wireshark/06-follow-tcp-stream.md)
*   [07-tls-handshake-inspection.md](wireshark/07-tls-handshake-inspection.md)
*   [08-mac-address-filter.md](wireshark/08-mac-address-filter.md)
*   [09-custom-tcp-flags-column.md](wireshark/09-custom-tcp-flags-column.md)
*   [10-detect-arp-spoofing.md](wireshark/10-detect-arp-spoofing.md)
*   [11-io-graph.md](wireshark/11-io-graph.md)
*   [12-top-talkers.md](wireshark/12-top-talkers.md)
*   [13-rogue-dhcp-server.md](wireshark/13-rogue-dhcp-server.md)
*   [14-http-user-agents.md](wireshark/14-http-user-agents.md)
*   [15-ioc-hunting.md](wireshark/15-ioc-hunting.md)
*   [16-encryption-verification.md](wireshark/16-encryption-verification.md)

---

## Sysinternals Suite

A collection of technical utilities provided by Microsoft that help you manage, troubleshoot, and diagnose your Windows systems and applications. Critical for endpoint analysis during incident response.

*   [01-find-fake-svchost.md](sysinternals/01-find-fake-svchost.md)
*   [02-disable-malware-auto-start.md](sysinternals/02-disable-malware-auto-start.md)
*   [03-file-lock-detective.md](sysinternals/03-file-lock-detective.md)
*   [04-monitor-process-activity.md](sysinternals/04-monitor-process-activity.md)
*   [05-remote-command.md](sysinternals/05-remote-command.md)
*   [06-verify-signatures.md](sysinternals/06-verify-signatures.md)
*   [07-check-service-permissions.md](sysinternals/07-check-service-permissions.md)
*   [08-alternate-data-streams.md](sysinternals/08-alternate-data-streams.md)
*   [09-run-as-another-user.md](sysinternals/09-run-as-another-user.md)
*   [10-analyse-memory-usage.md](sysinternals/10-analyse-memory-usage.md)
*   [11-spot-suspicious-connections.md](sysinternals/11-spot-suspicious-connections.md)
*   [12-check-loaded-dlls.md](sysinternals/12-check-loaded-dlls.md)
*   [13-baseline-autoruns.md](sysinternals/13-baseline-autoruns.md)
*   [14-capture-a-registry-change.md](sysinternals/14-capture-a-registry-change.md)
*   [15-gather-remote-system-info.md](sysinternals/15-gather-remote-system-info.md)
*   [16-monitor-disk-for-ransomware.md](sysinternals/16-monitor-disk-for-ransomware.md)

---

## Nmap

A free and open-source utility for network discovery and security auditing. It's used to discover hosts and services on a computer network by sending packets and analyzing their responses. Fundamental for reconnaissance and understanding network attack surfaces.

*   [01-discover-live-hosts.md](nmap/01-discover-live-hosts.md)
*   [02-scan-common-ports.md](nmap/02-scan-common-ports.md)
*   [03-service-version-detection.md](nmap/03-service-version-detection.md)
*   [04-os-fingerprinting.md](nmap/04-os-fingerprinting.md)
*   [05-aggressive-scan.md](nmap/05-aggressive-scan.md)
*   [06-udp-port-scan.md](nmap/06-udp-port-scan.md)
*   [07-scan-port-ranges.md](nmap/07-scan-port-ranges.md)
*   [08-output-formats.md](nmap/08-output-formats.md)
*   [09-nmap-scripting-engine-nse.md](nmap/09-nmap-scripting-engine-nse.md)
*   [10-vulnerability-scanning-nse.md](nmap/10-vulnerability-scanning-nse.md)
*   [11-timing-templates.md](nmap/11-timing-templates.md)
*   [12-evade-firewalls-fragmentation.md](nmap/12-evade-firewalls-fragmentation.md)
*   [13-decoy-scan.md](nmap/13-decoy-scan.md)
*   [14-idle-scan.md](nmap/14-idle-scan.md)
*   [15-target-from-file.md](nmap/15-target-from-file.md)
*   [16-http-enum-nse.md](nmap/16-http-enum-nse.md)

---

## Python

A versatile programming language widely used in cybersecurity for scripting, automation, data analysis, and developing custom security tools. Essential for enhancing efficiency in THIR workflows.

*   [01-basic-socket-scanner.md](python/01-basic-socket-scanner.md)
*   [02-parse-nmap-xml-lxml.md](python/02-parse-nmap-xml-lxml.md)
*   [03-simple-http-client.md](python/03-simple-http-client.md)
*   [04-file-hash-calculator.md](python/04-file-hash-calculator.md)
*   [05-log-file-parser-regex.md](python/05-log-file-parser-regex.md)
*   [06-platform-process-info.md](python/06-platform-process-info.md)
*   [07-platform-network-info.md](python/07-platform-network-info.md)
*   [08-create-simple-web-server.md](python/08-create-simple-web-server.md)
*   [09-email-alert-script.md](python/09-email-alert-script.md)
*   [10-read-write-csv.md](python/10-read-write-csv.md)
*   [11-os-command-execution.md](python/11-os-command-execution.md)
*   [12-env-var-manipulation.md](python/12-env-var-manipulation.md)
*   [13-file-integrity-monitor.md](python/13-file-integrity-monitor.md)
*   [14-regex-for-ioc-extraction.md](python/14-regex-for-ioc-extraction.md)
*   [15-data-encoding-decoding.md](python/15-data-encoding-decoding.md)
*   [16-json-api-interaction.md](python/16-json-api-interaction.md)

---

## Elastic Stack (ELK)

Comprises Elasticsearch (a search and analytics engine), Kibana (a visualization and dashboard tool), and Beats/Logstash (data shippers and processors). It's a powerful platform for centralizing, analyzing, and visualizing logs and metrics for threat detection and incident investigation.

*   [01-install-kibana-dev-tools.md](elastic-stack/01-install-kibana-dev-tools.md)
*   [02-ingest-log-file-beats.md](elastic-stack/02-ingest-log-file-beats.md)
*   [03-search-data-kibana.md](elastic-stack/03-search-data-kibana.md)
*   [04-create-index-pattern.md](elastic-stack/04-create-index-pattern.md)
*   [05-visualize-data-lens.md](elastic-stack/05-visualize-data-lens.md)
*   [06-build-dashboard.md](elastic-stack/06-build-dashboard.md)
*   [07-ingest-csv-logstash.md](elastic-stack/07-ingest-csv-logstash.md)
*   [08-enrich-data-logstash-geoip.md](elastic-stack/08-enrich-data-logstash-geoip.md)
*   [09-custom-kibana-dashboard-import.md](elastic-stack/09-custom-kibana-dashboard-import.md)
*   [10-alerting-watcher.md](elastic-stack/10-alerting-watcher.md)
*   [11-monitor-es-cluster-stack.md](elastic-stack/11-monitor-es-cluster-stack.md)
*   [12-ingest-web-logs-nginx.md](elastic-stack/12-ingest-web-logs-nginx.md)
*   [13-data-views-kibana.md](elastic-stack/13-data-views-kibana.md)
*   [14-security-event-detection.md](elastic-stack/14-security-event-detection.md)
*   [15-es-query-dsl-basics.md](elastic-stack/15-es-query-dsl-basics.md)
*   [16-ingest-syslog-beats.md](elastic-stack/16-ingest-syslog-beats.md)

---

## NIST SP 800-61

The National Institute of Standards and Technology (NIST) Special Publication 800-61, "Computer Security Incident Handling Guide," provides guidelines for handling computer security incidents. It offers a structured framework for effective incident response across all organizational types.

*   [01-understand-nist-framework.md](nist-sp-800-61/01-understand-nist-framework.md)
*   [02-incident-response-policy.md](nist-sp-800-61/02-incident-response-policy.md)
*   [03-incident-categorization.md](nist-sp-800-61/03-incident-categorization.md)
*   [04-preparation-phase-checklists.md](nist-sp-800-61/04-preparation-phase-checklists.md)
*   [05-detection-analysis-indicators.md](nist-sp-800-61/05-detection-analysis-indicators.md)
*   [06-containment-strategies.md](nist-sp-800-61/06-containment-strategies.md)
*   [07-eradication-recovery-steps.md](nist-sp-800-61/07-eradication-recovery-steps.md)
*   [08-post-incident-lessons-learned.md](nist-sp-800-61/08-post-incident-lessons-learned.md)
*   [09-communication-plan.md](nist-sp-800-61/09-communication-plan.md)
*   [10-forensic-data-collection.md](nist-sp-800-61/10-forensic-data-collection.md)
*   [11-threat-intelligence-integration.md](nist-sp-800-61/11-threat-intelligence-integration.md)
*   [12-security-awareness-training.md](nist-sp-800-61/12-security-awareness-training.md)
*   [13-legal-regulatory-reporting.md](nist-sp-800-61/13-legal-regulatory-reporting.md)
*   [14-tabletop-exercise-scenario.md](nist-sp-800-61/14-tabletop-exercise-scenario.md)
*   [15-metrics-reporting.md](nist-sp-800-61/15-metrics-reporting.md)
*   [16-incident-response-team-roles.md](nist-sp-800-61/16-incident-response-team-roles.md)
