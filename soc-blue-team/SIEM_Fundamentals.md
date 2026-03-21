# 📊 SIEM Fundamentals

> SIEM is the most important tool in any SOC. Every L1 analyst spends the majority of their day working inside a SIEM. Understanding what SIEM is, how it works, and how to use it is the single most critical skill for landing an entry-level SOC role.

---

## 📋 Table of Contents
- [What is SIEM?](#what-is-siem)
- [Why SIEM Exists](#why-siem-exists)
- [How SIEM Works](#how-siem-works)
- [Log Sources](#log-sources)
- [SIEM Key Features](#siem-key-features)
- [SIEM Use Cases](#siem-use-cases)
- [Popular SIEM Platforms](#popular-siem-platforms)
- [Splunk Overview](#splunk-overview)
- [Security Relevance](#security-relevance)
- [Quick Reference](#quick-reference)

---

## What is SIEM?

**SIEM** stands for **Security Information and Event Management**.

It is a platform that:
- **Collects** log data from across an entire organisation's IT infrastructure
- **Aggregates** it all into one central location
- **Correlates** events to identify patterns and suspicious activity
- **Alerts** analysts when something looks like a threat
- **Stores** logs for investigation and compliance purposes

In simple terms — SIEM is the **central nervous system of a SOC**. Every device, server, application, and network component sends its logs to the SIEM. Analysts use the SIEM to search, investigate, and detect threats.

---

## Why SIEM Exists

A modern organisation might have:
- Hundreds of servers
- Thousands of endpoints (laptops, desktops)
- Dozens of network devices (routers, switches, firewalls)
- Multiple cloud environments
- Various applications

Each of these generates thousands of log entries per day. A single analyst cannot manually review all of them. SIEM solves this by:

- **Centralising** all logs in one place
- **Automating** detection through correlation rules
- **Prioritising** alerts so analysts focus on what matters
- **Providing** search and investigation tools

---

## How SIEM Works

```
Step 1: LOG COLLECTION
        Devices send logs to SIEM via agents or syslog
        ┌─────────────┐
        │ Firewall    │──┐
        │ Web Server  │──┤
        │ Endpoints   │──┼──► SIEM (Central Platform)
        │ Active Dir  │──┤
        │ Cloud Apps  │──┘
        └─────────────┘

Step 2: LOG NORMALISATION
        Logs arrive in different formats
        SIEM converts them all to a standard format
        so they can be compared and correlated

Step 3: CORRELATION
        SIEM applies rules to find patterns across logs
        Example rule: "If the same IP fails login 10 times
        in 5 minutes across any system → trigger alert"

Step 4: ALERTING
        When a rule matches → alert is created
        Alert appears on analyst's dashboard

Step 5: INVESTIGATION
        Analyst reviews the alert
        Searches logs for more context
        Determines if it is a true or false positive

Step 6: RESPONSE
        Confirmed threat → escalate and contain
        False positive → document and tune the rule
```

---

## Log Sources

SIEM collects logs from many different sources:

### Network Logs
| Source | What it logs |
|---|---|
| Firewall | Allowed and blocked connections, port scans |
| Router/Switch | Network traffic, routing changes |
| IDS/IPS | Intrusion detection alerts |
| DNS Server | All DNS queries and responses |
| Proxy Server | Web browsing activity |

### Endpoint Logs
| Source | What it logs |
|---|---|
| Windows Event Logs | Logins, process creation, file access, registry changes |
| Linux Syslog | System events, authentication, service activity |
| Antivirus/EDR | Malware detections, suspicious process activity |

### Application Logs
| Source | What it logs |
|---|---|
| Web Server | HTTP requests, errors, access attempts |
| Database | Queries, login attempts, data access |
| Email Server | Emails sent/received, spam, phishing attempts |
| Active Directory | User authentication, group changes, admin actions |

### Cloud Logs
| Source | What it logs |
|---|---|
| AWS CloudTrail | API calls, resource changes, IAM activity |
| Azure Monitor | Azure resource activity and security events |
| Google Cloud Audit | GCP activity and access logs |

---

## SIEM Key Features

### 1. Log Aggregation
Collects logs from all sources into one searchable platform. Without this, analysts would need to log into each system individually.

### 2. Correlation Rules
Automated rules that trigger alerts when suspicious patterns are detected across multiple log sources.

**Example correlation rule:**
```
IF:
  Same source IP attempts login on 3 different systems
  AND login fails more than 5 times each
  AND all within 10 minutes
THEN:
  Trigger alert: "Potential brute force attack"
  Severity: HIGH
```

### 3. Dashboards
Visual displays showing the current security state — number of alerts, top threat sources, geographic maps of attack origins, trending events.

### 4. Search and Investigation
Analysts can search across all logs using queries. Example in Splunk:
```
index=firewall src_ip=192.168.1.100 action=blocked
| stats count by dest_port
| sort -count
```
This shows all blocked connections from a suspicious IP sorted by destination port.

### 5. Alerting and Notification
When a correlation rule matches, the SIEM creates an alert and notifies the analyst — via dashboard, email, or ticketing system.

### 6. Log Retention
Stores logs for months or years for forensic investigation and compliance requirements. Many regulations (GDPR, PCI-DSS, HIPAA) require log retention for specific periods.

### 7. Threat Intelligence Integration
SIEM can integrate with threat intelligence feeds — automatically flagging known malicious IPs, domains, and file hashes that appear in logs.

---

## SIEM Use Cases

| Use Case | How SIEM Detects It |
|---|---|
| **Brute Force Attack** | Multiple failed logins from same IP in short time |
| **Insider Threat** | Employee accessing unusual files or systems outside work hours |
| **Malware Infection** | Endpoint making connections to known C2 servers |
| **Data Exfiltration** | Large outbound data transfers to unknown external IPs |
| **Privilege Escalation** | User account suddenly gaining admin privileges |
| **Lateral Movement** | Internal connections between systems that don't normally communicate |
| **Phishing Success** | User clicking a malicious link → endpoint making suspicious outbound connection |
| **DNS Tunneling** | Unusually high volume of DNS queries with long domain names |
| **Port Scanning** | Single IP connecting to many different ports in rapid succession |

---

## Popular SIEM Platforms

| Platform | Vendor | Common in |
|---|---|---|
| **Splunk** | Splunk Inc. | Most widely used — industry standard |
| **IBM QRadar** | IBM | Large enterprises |
| **Microsoft Sentinel** | Microsoft | Azure cloud environments |
| **Elastic SIEM** | Elastic | Open source, growing fast |
| **LogRhythm** | LogRhythm | Mid-size enterprises |
| **ArcSight** | Micro Focus | Government and large enterprises |

**For job hunting:** Splunk is the most in-demand SIEM skill on job postings. Learning Splunk = most valuable SIEM certification for entry-level roles.

---

## Splunk Overview

Splunk is the most widely used SIEM platform. It collects, indexes, and searches machine-generated data.

### Key Splunk Concepts

| Concept | Meaning |
|---|---|
| **Index** | Where logs are stored — like a database table |
| **Source** | Where the log came from (e.g. firewall, web server) |
| **Source Type** | Format of the log data |
| **Event** | A single log entry |
| **Field** | A specific piece of data extracted from an event (e.g. src_ip, dest_port) |
| **Search Processing Language (SPL)** | Splunk's query language |

### Basic SPL Queries

```splunk
# Search all events in an index
index=main

# Search for a specific IP
index=firewall src_ip="192.168.1.100"

# Count events by source
index=main | stats count by source

# Find failed logins
index=windows EventCode=4625
| stats count by src_ip
| sort -count

# Find top 10 destination IPs
index=network
| top limit=10 dest_ip
```

### Splunk Search Flow
```
Data Input → Indexing → Search → Visualise → Alert
```

---

## Security Relevance

### How Attackers Try to Evade SIEM

Understanding SIEM evasion helps analysts tune better detection rules:

- **Low and slow attacks** — attackers spread activity over long periods to stay below alert thresholds
- **Living off the land** — using legitimate built-in tools (PowerShell, WMI) that look normal in logs
- **Log deletion** — attackers try to delete logs after compromise to remove evidence
- **Timestomping** — modifying file timestamps to confuse forensic timelines

### How SOC Uses SIEM to Catch Attacks

| Attack | SIEM Detection |
|---|---|
| Brute Force | Multiple EventCode 4625 (failed login) from same IP |
| Privilege Escalation | EventCode 4672 (admin login) for non-admin account |
| Lateral Movement | SMB connections between internal hosts |
| C2 Communication | DNS queries to newly registered domains |
| Data Exfiltration | Large outbound transfers to unknown IPs at unusual hours |

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| SIEM stands for | Security Information and Event Management |
| Main purpose | Centralise logs, correlate events, alert analysts |
| Log aggregation | Collect logs from all sources into one place |
| Correlation rule | Automated pattern matching that triggers alerts |
| True positive | Real threat confirmed by investigation |
| False positive | Normal activity incorrectly flagged |
| Alert fatigue | Analysts overwhelmed by too many alerts |
| Splunk | Most widely used SIEM — industry standard |
| SPL | Splunk Processing Language — used to search logs |
| Index | Where Splunk stores log data |
| MTTD | Mean Time to Detect |
| MTTR | Mean Time to Respond |
| Log retention | Storing logs for forensics and compliance |
| Threat intel integration | Auto-flagging known malicious IPs/domains in logs |

---

*Part of the [SOC / Blue Team](./README.md) section of my cybersecurity learning journey.*
