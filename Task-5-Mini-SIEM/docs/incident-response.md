# Incident Response Playbook
## Mini SIEM – Linux Security Monitoring and SSH Threat Detection

**Project:** Mini SIEM using ELK Stack
**Monitoring Platform:** Elasticsearch, Logstash and Kibana
**Log Collection:** Filebeat
**Monitored Systems:** Arch Linux and Kali Linux
**Detection Focus:** SSH Authentication Failures
**Document Type:** Incident Response Procedure

---

## 1. Introduction

Linux servers are frequently accessed through SSH because it provides a convenient way to manage systems remotely. However, SSH is also a common target for unauthorized access attempts.

An attacker may repeatedly try different usernames and passwords in an attempt to gain access to a system. This activity is commonly associated with password guessing and brute-force attacks.

The purpose of this Mini SIEM project is to collect Linux system logs, identify suspicious authentication activity and make those events easier to investigate through a centralized monitoring platform.

This playbook explains how an analyst should respond when the Mini SIEM detects repeated SSH authentication failures.

It is designed around the actual capabilities implemented in this project rather than assuming that automated response features already exist.

## 2. Scope and Objectives

This procedure covers:

- Monitoring Linux authentication events.
- Identifying failed SSH login attempts.
- Extracting usernames and source IP addresses.
- Detecting repeated authentication failures.
- Investigating suspicious activity through Kibana.
- Preserving relevant evidence.
- Recommending appropriate containment and recovery actions.

The primary objectives are:

1. Identify potentially unauthorized SSH access attempts.
2. Understand which system and account are being targeted.
3. Determine whether the activity represents a genuine security concern.
4. Preserve sufficient evidence for further investigation.
5. Recommend appropriate actions to reduce the risk of unauthorized access.

## 3. Monitoring Architecture

The Mini SIEM uses the Elastic Stack to collect, process, store and visualize Linux security events.

### Components

**Filebeat**

Filebeat runs on monitored Linux machines. It collects systemd journal events and forwards them to Logstash.

**Logstash**

Logstash receives events through its Beats input on port 5044. It processes incoming records and identifies failed SSH authentication messages.

The configured Grok pattern extracts:
- Targeted username
- Source IP address
- Source port

It also adds structured event fields and the `ssh-auth-failure` detection tag.

**Elasticsearch**

Elasticsearch stores the processed events in daily indices following this pattern:

`linux-logs-YYYY.MM.dd`

**Kibana**

Kibana provides the interface used to search, investigate and visualize collected security events.

The dashboard includes:
- Failed SSH login timeline
- Top SSH failed-login source IPs
- Top targeted usernames
- Total failed SSH attempts

## 4. SSH Failed Login Detection

The current detection rule is named:

`SSH Failed Login Detection`

### Detection configuration

| Parameter | Configuration |
|---|---|
| Data view | Linux Security Logs |
| Index pattern | `linux-logs-*` |
| Detection query | `tags : "ssh-auth-failure"` |
| Aggregation | Count of matching documents |
| Threshold | Above 4 |
| Time window | 5 minutes |
| Execution interval | 1 minute |

### How detection works

When a Linux system records a failed SSH password authentication, Filebeat forwards the event to Logstash.

Logstash checks whether the message matches the expected failed-password format.

If the message is successfully parsed, the event receives the following fields:

- `event.category`: authentication
- `event.action`: ssh-login-failed
- `event.outcome`: failure
- `user.name`: targeted username
- `source.ip`: source IP address
- `source.port`: source port
- `tags`: ssh-auth-failure

Kibana evaluates the detection query against the collected events.

When more than four matching events are found within five minutes, the configured rule condition is satisfied.

This is a threshold-based detection mechanism. It does not independently establish that an attacker is responsible.

## 5. Initial Alert Assessment

When the detection rule generates an alert, the analyst should first understand what actually happened before taking action.

The presence of multiple failed login attempts does not automatically mean that a system has been compromised.

Possible explanations include:

- An individual repeatedly entering an incorrect password.
- An administrator using an outdated SSH credential.
- An automated service attempting authentication with an expired password.
- A password-guessing attempt from an unauthorized source.
- A legitimate security testing activity.

The analyst should identify the source, targeted account, affected machine and event timestamps.

### Initial questions

1. Which machine recorded the failed authentication events?
2. Which source IP generated the attempts?
3. Which username was targeted?
4. How many failures occurred?
5. Did the attempts happen within a short period?
6. Was there a successful login after the failures?
7. Is the source IP expected or recognized?

These questions help distinguish routine authentication mistakes from potentially malicious behaviour.

## 6. Investigation Procedure

### Step 1: Open the alert

Open Kibana and navigate to:

Observability → Alerts

Locate the SSH Failed Login Detection rule and inspect its alert information.

Record the alert timestamp and the relevant execution details.

### Step 2: Search for matching events

Navigate to:

Analytics → Discover

Select the `Linux Security Logs` data view.

Use the following KQL query:

`tags : "ssh-auth-failure"`

This displays events identified by the SSH failure detection pipeline.

### Step 3: Identify the affected system

Inspect:

`host.name`

This identifies the Linux machine that generated the authentication event.

For example:

`kali`

### Step 4: Identify the targeted username

Inspect:

`user.name`

This identifies the username involved in the failed authentication attempt.

An unfamiliar or privileged account may require additional investigation.

### Step 5: Identify the source IP

Inspect:

`source.ip`

This identifies the address from which the SSH connection originated.

Multiple failed attempts from one source may indicate password guessing.

However, the source IP alone does not establish the identity of the person responsible.

### Step 6: Review timestamps

Inspect:

`@timestamp`

Determine whether the authentication failures occurred within a short period or were distributed over a longer interval.

A high frequency of failures within a short period may indicate automated activity.

### Step 7: Check for successful authentication

The analyst should also review the authentication logs for successful SSH logins around the same time.

The objective is to determine whether an unsuccessful login sequence was followed by a successful authentication.

A successful login after repeated failures requires closer investigation, particularly if the account or source is unfamiliar.

### Step 8: Document findings

Record the findings in an incident investigation record.

At minimum, include:

- Incident reference
- Detection rule name
- Affected hostname
- Source IP address
- Targeted username
- First and last observed timestamps
- Number of failed attempts
- Related successful login activity
- Investigation findings
- Recommended response

## 7. Incident Severity Assessment

The following classification can be used as an initial guide.

### Low Severity

Examples:
- A small number of failed login attempts.
- A known administrator entering an incorrect password.
- An expected authentication issue.

Recommended response:
- Record the event.
- Monitor for repeated activity.
- Confirm whether the activity was legitimate.

### Medium Severity

Examples:
- Repeated failures against one account.
- Multiple usernames targeted from one source.
- Unfamiliar source IP addresses.
- Repeated activity across monitored machines.

Recommended response:
- Investigate the source and targeted accounts.
- Review surrounding authentication events.
- Notify the responsible system administrator.
- Continue monitoring.

### High Severity

Examples:
- Repeated authentication failures followed by a successful login.
- Suspicious access to a privileged account.
- Evidence of unauthorized access.
- Authentication activity associated with other suspicious system events.

Recommended response:
- Escalate to the security or infrastructure team.
- Preserve relevant logs.
- Investigate the affected account and host.
- Consider containment measures with appropriate authorization.

Severity should be assigned using the available evidence, not simply the number of failed logins.

## 8. Containment and Response

If the investigation indicates suspicious activity, the responsible administrator should consider appropriate containment measures.

Possible actions include:

1. Restricting the suspicious source IP using approved firewall controls.
2. Reviewing SSH access policies.
3. Disabling or temporarily restricting a compromised account.
4. Rotating credentials if compromise is suspected.
5. Reviewing privileged account access.
6. Preserving authentication logs for further investigation.

Any containment action must be authorized and should account for the possibility of blocking legitimate users.

**Important:** The current Mini SIEM does not automatically block IP addresses or disable user accounts. These are manual response recommendations.

## 9. Recovery and Post-Incident Review

After the suspicious activity has been investigated and contained, the administrator should verify that the affected system is operating normally.

The recovery process should include:

- Confirming that unauthorized access has been stopped.
- Reviewing account credentials and access permissions.
- Checking SSH configuration.
- Monitoring authentication activity for repeated attempts.
- Confirming that log collection is functioning.
- Recording the actions taken during the investigation.

A post-incident review should identify what happened, why it happened and whether additional monitoring or security controls are required.

## 10. Evidence Collection and Preservation

Evidence should be collected before making changes that could remove or overwrite relevant information.

The following information should be preserved:

| Evidence | Purpose |
|---|---|
| Original authentication logs | Establish what happened |
| Kibana event records | Support investigation |
| Source IP address | Identify connection origin |
| Targeted username | Identify affected account |
| Event timestamps | Establish sequence of activity |
| Alert details | Record detection information |
| Investigation screenshots | Provide visual evidence |
| Response actions | Maintain an incident record |

The original event data should be retained wherever possible.

Screenshots are useful for reporting, but they should not replace the underlying log records.

## 11. Validation Performed During This Project

The detection pipeline was tested using a controlled SSH authentication failure on the Kali Linux virtual machine.

The test used a localhost SSH connection.

### Observed event

Example extracted information:

| Field | Observed value |
|---|---|
| Host | kali |
| Username | invaliduser |
| Source IP | 127.0.0.1 |
| Source port | 36256 |
| Event action | ssh-login-failed |
| Event category | authentication |
| Event outcome | failure |
| Detection tag | ssh-auth-failure |

The event was successfully:
1. Generated by the SSH service.
2. Collected through journald using Filebeat.
3. Forwarded to Logstash.
4. Parsed using the configured Grok pattern.
5. Stored in Elasticsearch.
6. Displayed in Kibana Discover.

The Kibana alert query was also tested and returned five matching documents within the configured five-minute window.

The threshold condition was configured to trigger above four matching events.

This validates the event-processing and threshold configuration in the controlled lab environment.

It does not demonstrate an actual external attack or successful compromise.

## 12. Current Project Limitations

The Mini SIEM is an educational prototype. Its current capabilities and limitations should be clearly understood.

Implemented:
- Centralized Linux log collection.
- Filebeat-to-Logstash event forwarding.
- Elasticsearch event storage.
- SSH failed-password event parsing.
- Username and source IP extraction.
- Kibana Discover investigation.
- Security monitoring dashboard.
- Threshold-based SSH detection rule.

Not currently implemented or fully verified:
- Automatic IP blocking.
- Automatic account disabling.
- Email or Slack notifications.
- Advanced correlation across different event types.
- MITRE ATT&CK automated mapping.
- Production-grade access control and encryption.
- Comprehensive external attack validation.

These capabilities may be considered future improvements.

## 13. Recommendations for Future Improvements

The following improvements could make the project more capable:

1. Add detection for successful SSH authentication.
2. Detect repeated failures across multiple usernames.
3. Add source IP reputation enrichment.
4. Integrate threat intelligence feeds.
5. Map relevant events to MITRE ATT&CK techniques.
6. Configure email or messaging notifications.
7. Add additional Linux authentication and system monitoring rules.
8. Implement controlled automated response with administrator approval.
9. Introduce role-based access control and secure transport.
10. Expand testing to multiple monitored Linux systems.

## 14. Conclusion

This Mini SIEM project demonstrates how Linux security logs can be collected, processed and monitored through a centralized Elastic Stack environment.

The implementation focuses on identifying repeated SSH authentication failures and extracting useful investigation information, including source IP addresses, usernames and affected hosts.

The detection rule provides a starting point for identifying suspicious authentication behaviour, while Kibana gives an analyst a practical interface for reviewing events.

Although the project does not provide a complete production-grade incident response platform, it demonstrates the core workflow of security monitoring: log collection, event processing, detection, investigation and response planning.

The playbook provides a consistent procedure for investigating suspicious SSH authentication activity and documenting the findings.

---

**Project:** Mini SIEM using ELK Stack
**Document:** Incident Response Playbook
**Environment:** Isolated Linux Virtual Lab
**Purpose:** Academic Capstone Project
