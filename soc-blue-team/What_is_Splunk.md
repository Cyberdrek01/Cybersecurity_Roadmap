# 📊 What is Splunk?

> Splunk is the most widely used SIEM platform in the industry and the most in-demand tool on SOC job postings. This file covers the fundamentals of what Splunk is, how it works, its architecture, and why it is essential for cybersecurity.

---

## 📋 Table of Contents
- [What is Splunk?](#what-is-splunk)
- [Why Splunk?](#why-splunk)
- [How Splunk Works](#how-splunk-works)
- [Splunk Architecture](#splunk-architecture)
- [Splunk Components](#splunk-components)
- [Splunk Web Interface](#splunk-web-interface)
- [Splunk Use Cases in SOC](#splunk-use-cases-in-soc)
- [Splunk vs Other SIEMs](#splunk-vs-other-siems)
- [Quick Reference](#quick-reference)

---

## What is Splunk?

**Splunk** is a data analytics and SIEM platform that collects, indexes, searches, and visualises machine-generated data from any source in real time.

In simple terms — Splunk is a **search engine for log data**. Just like Google searches the web, Splunk searches through millions of log entries across your entire IT infrastructure and finds exactly what you are looking for.

**Splunk in one sentence:**
> Splunk collects logs from everything, stores them in one place, lets you search through them instantly, and alerts you when something looks suspicious.

---

## Why Splunk?

Modern organisations generate massive amounts of data:
- Servers generate thousands of log entries per minute
- Firewalls log every connection attempt
- Applications log every user action
- Endpoints log every process and file access

Without a tool like Splunk, finding a single suspicious event in millions of logs would take days. Splunk makes it take seconds.

**Why Splunk specifically:**
- Used by over 15,000 organisations worldwide
- Most mentioned SIEM on cybersecurity job postings
- Has its own certification — **Splunk Core Certified User**
- Free tier available for learning and small deployments
- Industry standard — knowing Splunk = employable

---

## How Splunk Works

```
Step 1: DATA COLLECTION
        Splunk collects data from any source:
        ┌─────────────┐
        │ Servers     │──┐
        │ Firewalls   │──┤
        │ Endpoints   │──┼──► Splunk Forwarder ──► Splunk Indexer
        │ Applications│──┤
        │ Cloud       │──┘
        └─────────────┘

Step 2: INDEXING
        Splunk breaks data into individual events
        Extracts fields automatically (IP, timestamp, user etc.)
        Stores everything in indexes for fast searching

Step 3: SEARCHING
        Analyst types a search query in SPL
        Splunk searches through indexed data instantly
        Returns matching events

Step 4: VISUALISING
        Results displayed as tables, charts, graphs
        Dashboards show real-time security status

Step 5: ALERTING
        Saved searches run on schedule
        If conditions match → alert sent to analyst
```

---

## Splunk Architecture

Splunk has three main tiers:

```
┌─────────────────────────────────────────────────┐
│              SEARCH HEAD                         │
│   Web interface — where analysts work            │
│   Run searches, view dashboards, manage alerts   │
├─────────────────────────────────────────────────┤
│              INDEXER                             │
│   Stores and indexes all log data                │
│   Processes search queries                       │
├─────────────────────────────────────────────────┤
│              FORWARDER                           │
│   Installed on source systems                    │
│   Collects and sends logs to the Indexer         │
└─────────────────────────────────────────────────┘
```

### Types of Forwarders

| Type | Purpose |
|---|---|
| **Universal Forwarder (UF)** | Lightweight — collects and forwards raw data, minimal processing |
| **Heavy Forwarder (HF)** | Full Splunk instance — can parse and filter data before forwarding |

Universal Forwarders are installed on every server and endpoint that needs to send logs to Splunk.

---

## Splunk Components

### Index
A storage container for data — like a database table. Different data types are stored in separate indexes:

```
index=windows    ← Windows event logs
index=linux      ← Linux syslog
index=firewall   ← Firewall logs
index=web        ← Web server logs
index=main       ← Default index
```

### Event
A single log entry — the basic unit of data in Splunk. Every line in a log file becomes one event.

**Example event:**
```
2026-03-20 12:15:33 src_ip=192.168.1.100 dest_ip=8.8.8.8 
dest_port=53 action=allowed protocol=UDP bytes=74
```

### Fields
Key-value pairs automatically extracted from events:
- `src_ip = 192.168.1.100`
- `dest_port = 53`
- `action = allowed`

Fields make searching and filtering fast and precise.

### Source and Source Type
| Term | Meaning | Example |
|---|---|---|
| **Source** | Where the data came from | `/var/log/auth.log` |
| **Source Type** | Format/type of the data | `linux_secure`, `WinEventLog` |
| **Host** | Which machine sent the data | `webserver-01` |

---

## Splunk Web Interface

The Splunk Web interface has several key sections:

### Search & Reporting App
The main workspace for analysts — where searches are run and results are viewed.

**Key areas:**
- **Search bar** — type SPL queries here
- **Time picker** — filter by time range (last 15 min, last 24h, custom)
- **Events tab** — shows raw log events matching the search
- **Statistics tab** — shows tabular results
- **Visualisation tab** — shows charts and graphs

### Dashboards
Pre-built or custom visual displays showing security metrics in real time. An L1 SOC analyst monitors dashboards at the start of every shift.

### Alerts
Saved searches that run automatically on a schedule. When conditions are met an alert is triggered and the analyst is notified.

### Reports
Saved searches that generate scheduled reports — used for management reporting and compliance.

---

## Splunk Use Cases in SOC

| Use Case | How Splunk Helps |
|---|---|
| **Brute Force Detection** | Alert when same IP has 10+ failed logins in 5 minutes |
| **Malware Detection** | Alert when endpoint connects to known malicious IP |
| **Data Exfiltration** | Alert on large outbound data transfers |
| **Insider Threat** | Detect employees accessing unusual files after hours |
| **Compliance Reporting** | Generate automated reports for auditors |
| **Incident Investigation** | Search all logs related to a suspicious IP or user |
| **Threat Hunting** | Search for indicators of compromise across all data |

---

## Splunk vs Other SIEMs

| SIEM | Strengths | Common in |
|---|---|---|
| **Splunk** | Most powerful search, huge community, best documentation | Most industries |
| **IBM QRadar** | Strong correlation, enterprise features | Large enterprises |
| **Microsoft Sentinel** | Native Azure integration, cloud-first | Microsoft environments |
| **Elastic SIEM** | Open source, flexible, cost-effective | Startups, mid-size |
| **LogRhythm** | Strong compliance features | Regulated industries |

**For job hunting in India:** Splunk and IBM QRadar are the most commonly mentioned in SOC job postings. Splunk certification carries the most weight for entry-level roles.

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| Splunk purpose | Collect, index, search, and visualise log data |
| Forwarder | Collects logs from source systems and sends to indexer |
| Indexer | Stores and processes log data |
| Search Head | Web interface where analysts run searches |
| Index | Storage container for specific data type |
| Event | Single log entry — basic unit of Splunk data |
| Field | Key-value pair extracted from an event |
| SPL | Splunk Processing Language — query language |
| Source Type | Format/type of incoming data |
| Dashboard | Visual display of real-time security metrics |
| Alert | Automated notification when search conditions are met |
| Universal Forwarder | Lightweight agent installed on source systems |

---

## 📚 Resources

| Resource | Link |
|---|---|
| Splunk Free Training | [splunk.com/en_us/training.html](https://www.splunk.com/en_us/training.html) |
| Splunk Documentation | [docs.splunk.com](https://docs.splunk.com) |
| Splunk Fundamentals 1 | Free certification course on Splunk training portal |

---

*Part of the [SOC / Blue Team](./README.md) section of my cybersecurity learning journey.*
