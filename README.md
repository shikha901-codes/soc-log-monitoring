# SOC Log Monitoring Dashboard using Splunk

## Project Overview
This project demonstrates basic Security Operations Center (SOC) log monitoring and analysis using Splunk Enterprise on Kali Linux.

The project collects Linux system journal logs, indexes them in Splunk, and uses Search Processing Language (SPL) to monitor system activity and identify authentication failures.

## Tools and Technologies
- Kali Linux
- Splunk Enterprise
- SPL (Search Processing Language)
- Linux system logs
- Linux Journal (systemd-journald)

## Project Structure
SOC-Log-Monitoring/
├── logs/
├── scripts/
├── splunk_queries/
├── screenshots/
├── documentation/
└── README.md

## Dashboard Panels
1. Privileged Session Activity
2. Total System Events
3. Sudo Activity
4. Sudo Activity by User
5. Authentication Failures

## Detection Use Case
Repeated Sudo Authentication Failures

The detection query identifies three or more failed sudo authentication attempts by the same username within a five-minute window.

## Example SPL Query

    index=main sourcetype=linux_journal "pam_unix(sudo:auth): authentication failure"
    | rex "user=(?<username>\S+)"
    | bin _time span=5m
    | stats count AS failed_attempts by _time username
    | where failed_attempts >= 3

## Current Findings
- Total indexed system events: 2604
- Sudo-related events: 41
- Failed sudo authentication events observed: 1
- Observed failed authentication username: shikha

## Alert
A Splunk alert named "Repeated Sudo Authentication Failures" was created.

Note: The alert is currently configured as real-time. Its scheduling configuration has not yet been optimized.

## Learning Outcomes
- Linux log monitoring
- Splunk data ingestion
- SPL query writing
- Authentication event analysis
- Basic SOC detection logic
- Dashboard creation and alert configuration

## Future Improvements
- Add more authentication detection use cases.
- Analyze SSH authentication events when available.
- Improve alert scheduling and testing.
- Add screenshots and project documentation.
