# ApexPlanet Cybersecurity Internship

A practical cybersecurity portfolio documenting hands-on projects, technical exercises, security assessments, configurations, and implementation work completed during the ApexPlanet Cybersecurity Internship.

This repository demonstrates practical learning across cybersecurity fundamentals, network security, web application security, system exploitation, and Security Information and Event Management (SIEM).

The projects are organized into individual tasks, with dedicated documentation, technical notes, reports, configurations, and supporting evidence.

---

## Internship Projects

| Task | Project | Domain |
|---|---|---|
| Task 01 | Foundations | Cybersecurity Fundamentals |
| Task 02 | Network Security | Network Reconnaissance & Network Defence |
| Task 03 | Web Application Security | Web Vulnerability Assessment |
| Task 04 | Exploitation & System Security | Penetration Testing & System Hardening |
| Task 05 | Mini SIEM using ELK Stack | Security Monitoring & Incident Detection |

---

## Task Details

### Task 01 — Foundations

**Focus:** Building foundational knowledge of cybersecurity, Linux, networking, cryptography, and security tools.

Topics and practical work:

- Cybersecurity fundamentals
- Linux fundamentals
- Networking concepts
- Cryptography
- Nmap
- Wireshark
- Burp Suite
- Netcat
- OpenSSL encryption and decryption

**Project structure:** `Task-1-Foundations/`

### Task 02 — Network Security

**Focus:** Understanding network reconnaissance, vulnerability scanning, packet analysis, and firewall configuration.

Topics and practical work:

- Passive reconnaissance
- Active reconnaissance
- Nmap scanning
- Vulnerability scanning
- Wireshark packet analysis
- Firewall configuration using iptables

**Project structure:** `Task-2-Network-Security/`

### Task 03 — Web Application Security

**Focus:** Understanding common web application vulnerabilities and security testing techniques.

Topics and practical work:

- SQL Injection
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- File Inclusion vulnerabilities
- Burp Suite
- HTTP Security Headers

**Project structure:** `Task-3-Web-Application-Security/`

### Task 04 — Exploitation & System Security

**Focus:** Practical penetration testing methodology, exploitation techniques, security awareness, malware fundamentals, and system hardening.

Completed sections:

| Section | Status |
|---|---|
| Penetration Testing Methodology | Completed |
| Metasploit Exploitation | Completed |
| Password Attacks | Completed |
| Social Engineering Simulation | Completed |
| Malware Basics | Completed |
| System Hardening | Completed |

Supporting project materials include:

- Methodology and reconnaissance documentation
- Metasploit assessment documentation
- Password attack exercises
- Social engineering simulation
- Malware analysis documentation
- System hardening documentation
- Reports and screenshots

**Project structure:** `Task-4-Exploitation-System-Security/`

### Task 05 — Mini SIEM using ELK Stack

**Focus:** Building a small SIEM environment for centralized Linux log collection, processing, monitoring, and SSH authentication failure detection.

**Technology stack:**

- **Filebeat:** Collects system logs from Arch Linux and Kali Linux.
- **Logstash:** Receives, processes, and parses incoming log events.
- **Elasticsearch:** Stores and indexes processed events.
- **Kibana:** Provides log investigation, dashboards, and alert monitoring.
- **Docker Compose:** Manages the ELK Stack deployment.

**Implemented capabilities:**

- Centralized log collection from multiple Linux systems.
- SSH authentication failure detection.
- Log parsing using Grok patterns.
- Extraction of usernames, source IP addresses, and authentication outcomes.
- Kibana Discover for event investigation.
- Security dashboard with failed-login timeline, source IPs, targeted usernames, and event counts.
- Threshold-based SSH failed-login detection rule.
- Incident response documentation and architecture diagrams.

**Project structure:** `Task-5-Mini-SIEM/`

---

## Technologies & Tools

| Category | Technologies |
|---|---|
| Operating Systems | Arch Linux, Kali Linux |
| Network Security | Nmap, Wireshark, Netcat |
| Web Application Security | Burp Suite |
| System Security | Metasploit |
| Cryptography | OpenSSL |
| SIEM & Log Management | Elasticsearch, Logstash, Kibana |
| Log Collection | Filebeat |
| Containerization | Docker, Docker Compose |
| Documentation | Markdown, Mermaid |
| Version Control | Git, GitHub |

---

## Repository Structure

```text
apexplanet-cybersecurity-internship/
│
├── README.md
│
├── Task-1-Foundations/
│   ├── README.md
│   ├── notes/
│   ├── tools/
│   ├── openssl/
│   ├── screenshots/
│   └── diagrams/
│
├── Task-2-Network-Security/
│   ├── README.md
│   ├── reconnaissance/
│   ├── nmap/
│   ├── vulnerability-scanning/
│   ├── wireshark/
│   ├── firewall/
│   ├── screenshots/
│   └── reports/
│
├── Task-3-Web-Application-Security/
│   ├── README.md
│   ├── sql-injection/
│   ├── xss/
│   ├── csrf/
│   ├── file-inclusion/
│   ├── burp-suite/
│   ├── security-headers/
│   ├── screenshots/
│   └── reports/
│
├── Task-4-Exploitation-System-Security/
│   ├── README.md
│   ├── malware-analysis/
│   ├── metasploit/
│   ├── methodology/
│   ├── password-attacks/
│   ├── reports/
│   ├── screenshots/
│   └── social-engineering/
│
└── Task-5-Mini-SIEM/
    ├── README.md
    ├── .gitignore
    ├── compose.yaml
    ├── docs/
    │   ├── architecture.md
    │   ├── architecture.mmd
    │   └── incident-response.md
    └── logstash/
        └── pipeline/
            └── logstash.conf
```

---

## Learning Outcomes

The internship projects focus on developing practical skills in:

- Linux system administration and command-line operations.
- Network reconnaissance and security analysis.
- Web application vulnerability identification.
- Penetration testing methodology and exploitation fundamentals.
- Security configuration and system hardening.
- Centralized log collection and processing.
- Security event monitoring and detection logic.
- Technical documentation and incident response procedures.

---

## Documentation & Evidence

Each task maintains its own documentation and supporting files.

Depending on the task, these include:

- Technical notes and explanations.
- Tool configurations and practical commands.
- Assessment reports and findings.
- Screenshots and supporting evidence.
- Architecture diagrams.
- Implementation and incident response documentation.

---

## Disclaimer

All security testing and practical exercises are intended for educational purposes and authorized environments only.

Testing against systems without explicit permission is not authorized.

The Mini SIEM implementation is an educational prototype and should not be considered a production-ready SIEM deployment.

---

## Author

**Vivek Kumar**

Cybersecurity | Threat Intelligence | OSINT | Security Monitoring

GitHub: [@VivekIntel](https://github.com/VivekIntel)

---

*This repository documents practical cybersecurity learning, technical experimentation, and project implementations completed during the ApexPlanet Cybersecurity Internship.*
