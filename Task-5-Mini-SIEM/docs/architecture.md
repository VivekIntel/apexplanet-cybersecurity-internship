# Mini SIEM – System Architecture

## 1. Overview

The Mini SIEM is a centralized Linux security monitoring platform built using the Elastic Stack.

Its purpose is to collect Linux system and authentication logs, process security-relevant events, store them centrally and make them available for investigation through Kibana.

The lab uses two Linux machines:
- Arch Linux – Host system
- Kali Linux – Monitored virtual machine

## 2. Architecture Components

### 2.1 Log Sources

Linux systems generate system and authentication events through systemd-journald.

The project currently monitors:
- Arch Linux system logs
- Kali Linux SSH authentication logs

### 2.2 Filebeat

Filebeat is installed on both Linux machines.

Its responsibilities:
- Read events from systemd-journald.
- Forward collected events to Logstash.
- Maintain its registry to track processed events.

Kali Filebeat forwards events to `192.168.122.1:5044`.

### 2.3 Logstash

Logstash runs as a Docker container on the Arch host.

Its responsibilities:
- Receive events through the Beats input.
- Identify SSH failed-password messages.
- Extract usernames, source IP addresses and source ports.
- Add structured authentication event fields.
- Forward processed events to Elasticsearch.

Input port: `5044`

### 2.4 Elasticsearch

Elasticsearch stores processed security events.

Index pattern:

`linux-logs-YYYY.MM.dd`

Elasticsearch makes the collected data searchable for investigation and visualization.

API port: `9200`

### 2.5 Kibana

Kibana provides the monitoring and investigation interface.

The project uses Kibana for:
- Discover event investigation.
- Failed SSH login timelines.
- Top source IP visualization.
- Targeted username visualization.
- Total failed SSH attempt metrics.
- Threshold-based detection rules.

Web interface port: `5601`

## 3. Event Processing Workflow

1. A Linux system generates an event.
2. systemd-journald records the event.
3. Filebeat reads the journal entry.
4. Filebeat forwards the event to Logstash.
5. Logstash checks the event message.
6. Matching SSH failed-password messages are parsed.
7. Logstash adds structured fields and detection tags.
8. Elasticsearch stores the processed event.
9. Kibana queries and visualizes the event.
10. The configured alert rule evaluates matching SSH failures.

## 4. SSH Detection Logic

The Logstash pipeline identifies messages matching:

`Failed password for ... from ... port ... ssh2`

It extracts:
- `user.name`
- `source.ip`
- `source.port`

It adds:
- `event.category = authentication`
- `event.action = ssh-login-failed`
- `event.outcome = failure`
- `tags = ssh-auth-failure`

Kibana uses the detection tag to identify failed SSH authentication events.

## 5. Network Information

| Component | Address / Port |
|---|---|
| Arch host Wi-Fi | 10.73.84.136 |
| Arch libvirt interface | 192.168.122.1 |
| Kali VM | 192.168.122.11 |
| Logstash Beats input | 5044 |
| Elasticsearch | 9200 |
| Kibana | 5601 |
| Logstash monitoring API | 9600 |

The Kali VM communicates with Logstash through the libvirt virtual network.

## 6. Deployment

The Elastic Stack services are deployed through Docker Compose on Arch Linux.

Services:
- Elasticsearch
- Logstash
- Kibana

Filebeat runs as a systemd service on each monitored Linux machine.

## 7. Security Considerations

The current environment is an isolated academic lab.

Elasticsearch security is disabled in the current Docker Compose configuration.

The services should not be exposed directly to an untrusted network.

For production deployment, additional controls would be required, including:
- Authentication and role-based access control.
- TLS encryption.
- Secure secret management.
- Network restrictions.
- Log retention and backup policies.

## 8. Limitations

This implementation is a prototype.

It currently focuses on Linux log collection and SSH failed-login detection. It does not implement automated blocking, endpoint isolation or advanced threat intelligence enrichment.

## 9. Conclusion

The architecture demonstrates a centralized security monitoring workflow using Filebeat, Logstash, Elasticsearch and Kibana.

It provides a foundation for collecting Linux events, identifying suspicious SSH authentication activity and investigating the resulting events through a unified interface.
