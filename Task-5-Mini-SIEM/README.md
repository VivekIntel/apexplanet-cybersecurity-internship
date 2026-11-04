# Mini SIEM – Linux Security Monitoring

A hands-on SIEM capstone project built using the Elastic Stack. It collects Linux system logs, processes authentication events and detects repeated SSH login failures.

## Project Objective

Build a centralized monitoring platform to collect, analyze and investigate Linux security events from multiple machines.

## Architecture

Arch Linux and Kali Linux generate system logs through systemd-journald. Filebeat forwards these logs to Logstash, which parses SSH authentication failures. Elasticsearch stores the events, and Kibana provides investigation dashboards and alerting.

Architecture source: [architecture.mmd](docs/architecture.mmd)

## Technology Stack

- Arch Linux – SIEM host
- Kali Linux – monitored VM
- Filebeat – log collection
- Logstash – event processing
- Elasticsearch – event storage
- Kibana – visualization and alerting
- Docker Compose – service deployment

## Implemented Features

- Centralized Linux log collection
- systemd-journald integration
- Logstash Beats input
- SSH failed-password event parsing
- Username and source IP extraction
- Structured authentication event fields
- Elasticsearch log indexing
- Kibana security dashboard
- SSH failed-login threshold rule

## Detection Rule

Rule name: SSH Failed Login Detection

- Query: tags : "ssh-auth-failure"
- Threshold: More than 4 matching events
- Time window: 5 minutes
- Execution interval: 1 minute

## Lab Environment

| Component | Address |
|---|---|
| Arch Linux | 192.168.122.1 |
| Kali Linux VM | 192.168.122.11 |
| Logstash | Port 5044 |
| Elasticsearch | Port 9200 |
| Kibana | Port 5601 |

## Project Structure

- compose.yaml – Elastic Stack services
- logstash/pipeline/logstash.conf – event processing
- docs/architecture.md – architecture documentation
- docs/architecture.mmd – diagram source
- docs/incident-response.md – incident response playbook

## Deployment

Start services:

    sudo docker compose up -d

Check services:

    sudo docker compose ps

Open Kibana:

    http://localhost:5601

Filebeat must be installed and configured separately on each monitored machine.

## Validation

A controlled failed SSH authentication attempt against the Kali VM's localhost SSH service was used to validate log collection and parsing.

The resulting events were stored in Elasticsearch and visualized in Kibana.

## Current Limitations

- Educational lab prototype, not production-ready.
- Detection currently focuses on SSH failed-password events.
- Automated IP blocking is not implemented.
- External notification integrations are not configured.
- Advanced threat intelligence enrichment is not implemented.

## Future Improvements

- Successful SSH login monitoring
- Brute-force detection by source IP
- MITRE ATT&CK mapping
- Threat intelligence enrichment
- Email notifications
- Additional Linux detection rules

## Disclaimer

Developed for educational and defensive security monitoring in an authorized lab environment.
