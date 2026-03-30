# 📊 Splunk Basics

> Splunk is the most widely used SIEM platform in the industry and the most in-demand tool on SOC job postings. This file covers Splunk fundamentals, SPL (Splunk Processing Language), and real SOC investigation scenarios practiced hands-on.

---

## 📋 Table of Contents
- [Mental Model — Why Splunk Exists](#mental-model--why-splunk-exists)
- [How Splunk Thinks](#how-splunk-thinks)
- [The 5 Default Fields](#the-5-default-fields)
- [SPL — Splunk Processing Language](#spl--splunk-processing-language)
- [Essential SPL Commands](#essential-spl-commands)
- [Real SOC Searches](#real-soc-searches)
- [Dashboards](#dashboards)
- [Alerts](#alerts)
- [SOC Investigation Scenarios](#soc-investigation-scenarios)
- [Quick Reference](#quick-reference)

---

## Mental Model — Why Splunk Exists

As an L1 SOC analyst you sit down at 9am. Your SIEM shows 47 overnight alerts. You need to investigate them. The ONE thing you need to do is **search** — find the relevant events, filter them, and understand what happened.

Everything in Splunk comes down to search. Master search — master Splunk.

---

## How Splunk Thinks

Every device in an organisation generates logs constantly:

```
2026-03-23 09:15:33 host=webserver-01 src_ip=192.168.1.100
dest_ip=8.8.8.8 action=blocked port=443 user=john
```

Splunk takes every log line and:

```
Step 1: BREAKS IT INTO AN EVENT
        One log line = one event

Step 2: EXTRACTS FIELDS AUTOMATICALLY
        src_ip=192.168.1.100
        action=blocked
        port=443
        user=john

Step 3: STORES IN AN INDEX
        Like a labelled filing cabinet

Step 4: MAKES IT SEARCHABLE INSTANTLY
        Find anything across millions of logs in seconds
```

You are not searching raw text — you are searching **structured events with fields.**

---

## The 5 Default Fields

Every single event in Splunk always has these 5 fields:

| Field | Meaning | Example |
|---|---|---|
| `_time` | When the event happened | 2026-03-23 09:15:33 |
| `host` | Which machine sent it | webserver-01 |
| `source` | Which file it came from | /var/log/auth.log |
| `sourcetype` | What format the data is | linux_secure |
| `index` | Which storage bucket | main |

Plus custom fields extracted from the log itself — `src_ip`, `dest_ip`, `action`, `user`, `port` etc.

---

## SPL — Splunk Processing Language

SPL is Splunk's search language. The logic is simple:

### Basic Search — Find a keyword
```splunk
failed
```
Returns every event containing the word "failed" across all logs.

### Search in a Specific Index
```splunk
index=main failed
```
Search only in the "main" index.

### Filter by a Specific Field
```splunk
index=main src_ip=192.168.1.100
```
Find all events where source IP is exactly 192.168.1.100.

### Combine Multiple Conditions
```splunk
index=main src_ip=192.168.1.100 action=blocked
```
Find events where IP is 192.168.1.100 AND action is blocked.

### Set Time Range
```splunk
index=main failed earliest=-24h latest=now
```
Search last 24 hours only. Always set a time range — searching all time is slow.

### The Pipe ( | )
The pipe takes search results and passes them to a command for further processing — exactly like Linux pipes:

```splunk
index=main failed | stats count by src_ip
```

1. Find all events containing "failed"
2. Count how many times each src_ip appears
3. Show as a table

**Result:**
```
src_ip            count
192.168.1.100     847
10.0.0.55         234
172.16.0.12       12
```
Instantly shows which IP has the most failures — likely a brute force attack.

---

## Essential SPL Commands

### `stats` — Count and summarise
```splunk
index=main | stats count by src_ip
index=main | stats count by host
index=main | stats sum(bytes_out) by dest_ip
```

### `table` — Show specific fields only
```splunk
index=main | table _time src_ip dest_ip action
```
Shows only the columns you care about — cleaner view.

### `sort` — Order results
```splunk
index=main | stats count by src_ip | sort -count
```
`-count` = sort descending — highest first.

### `where` — Filter results after a pipe
```splunk
index=main | stats count by src_ip | where count > 100
```
Show only IPs that appear more than 100 times.

### `head` / `tail` — Limit results
```splunk
index=main | head 10
```
Show only first 10 events.

### `eval` — Create new calculated fields
```splunk
index=main | eval hour=strftime(_time, "%H")
```
Extracts the hour from the timestamp — useful for detecting after-hours activity.

---

## Real SOC Searches

### Detect Brute Force Attack
```splunk
index=windows EventCode=4625
| stats count by src_ip
| where count > 10
| sort -count
```
EventCode 4625 = Windows failed login. Finds IPs with more than 10 failed logins.

### Find All Activity From a Suspicious IP
```splunk
index=* src_ip=192.168.1.100
| table _time host action dest_ip dest_port
| sort _time
```
Shows everything that IP did — full chronological timeline.

### Detect After Hours Logins
```splunk
index=windows EventCode=4624
| eval hour=strftime(_time, "%H")
| where hour < 6 OR hour > 22
| table _time user host
```
EventCode 4624 = successful login. Shows logins between 10pm and 6am.

### Find Large Outbound Data Transfers
```splunk
index=network
| where bytes_out > 100000000
| table _time src_ip dest_ip bytes_out
| sort -bytes_out
```
Finds outbound transfers over 100MB — possible data exfiltration.

### Find Failed Logins by Username
```splunk
index=main failed earliest=-24h latest=now
| stats count by user
| sort -count
```
Shows which usernames are being targeted most.

---

## Dashboards

A dashboard is a collection of saved searches displayed visually in real time. Every SOC has dashboards that analysts monitor at the start of each shift.

**A typical SOC dashboard:**
```
┌──────────────┬──────────────┬──────────────┐
│ Total Alerts │Critical Alerts│Open Incidents│
│    1,247     │      23      │      8       │
├──────────────┴──────────────┴──────────────┤
│ Top 10 Source IPs (bar chart)              │
│ ████████████ 203.0.113.55 — 847           │
│ ████████ 10.0.0.22 — 234                 │
├────────────────────────────────────────────┤
│ Failed Logins Over Time (line chart)       │
│      ___                                   │
│ ____/   \____spike at 02:14am             │
└────────────────────────────────────────────┘
```

**Creating a dashboard panel:**
1. Run your search
2. Click **Save As → Dashboard Panel**
3. Name it and choose visualisation type
4. Add to existing or new dashboard

---

## Alerts

Alerts are saved searches that run automatically on a schedule and notify you when conditions are met.

**How to create an alert:**
1. Write your search
2. Click **Save As → Alert**
3. Set schedule — every 5 minutes, hourly, daily
4. Set trigger condition — when results > 0
5. Set action — send email, create ticket, run script

**Example — Brute Force Alert:**
```splunk
index=windows EventCode=4625
| stats count by src_ip
| where count > 10
```
- Schedule: every 5 minutes
- Trigger: when results > 0
- Action: email SOC team immediately

---

## SOC Investigation Scenarios

### Scenario 1 — Brute Force Attack

**Alert received:**
> HIGH SEVERITY — Multiple failed login attempts
> Source IP: 203.0.113.55 | Target: webserver-01 | Time: 02:14:33

**Step 1 — Find all activity from suspicious IP:**
```splunk
index=main src_ip=203.0.113.55 earliest=-24h latest=now
```

**Step 2 — Count failed logins by targeted username:**
```splunk
index=main src_ip=203.0.113.55 action=failure earliest=-24h latest=now
| stats count by username
| sort -count
```

**Step 3 — Check for successful login (did brute force work?):**
```splunk
index=main src_ip=203.0.113.55 action=success earliest=-24h latest=now
| table _time user host
```

**Finding:** 847 failed logins + 1 successful login at 02:17am

**Response:**
```
1. CONFIRM   → Brute force succeeded — account compromised
2. ESCALATE  → Report to L2/Team Lead with full findings
3. CONTAIN   → Block IP 203.0.113.55 at firewall
               Disable compromised account immediately
4. INVESTIGATE → What did attacker do after 02:17am?
                 Any new accounts created?
                 Any privilege escalation?
                 Any outbound connections?
5. DOCUMENT  → Full incident timeline and report
```

---

### Scenario 2 — Data Exfiltration

**Alert received:**
> HIGH SEVERITY — Possible data exfiltration detected
> Internal IP: 192.168.1.55 | Volume: 2GB | Time: 03:00am

**Step 1 — Find all outbound connections from suspicious IP:**
```splunk
index=main src_ip=192.168.1.55 earliest=-1h
| table _time dest_ip dest_port bytes_out
```

**Step 2 — Total bytes transferred by destination IP:**
```splunk
index=network src_ip=192.168.1.55 earliest=-1h
| stats sum(bytes_out) by dest_ip
| sort -bytes_out
```

**Finding:** 2GB sent to 185.220.101.45 at 3am

**Response:**
```
1. CONFIRM   → 2GB outbound to unknown external IP at 3am
               = Data Exfiltration confirmed
2. ESCALATE  → Report to L2/Team Lead immediately:
               Source: 192.168.1.55
               Destination: 185.220.101.45
               Volume: 2GB | Time: 3am
3. CONTAIN   → Block 192.168.1.55 outbound connections
               Block 185.220.101.45 at firewall
4. INVESTIGATE → How did attacker get in?
                 When did 192.168.1.55 first appear?
                 What files were accessed before 3am?
                 Any logins before the exfiltration?
5. DOCUMENT  → Full incident report with timeline
```

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| Event | Single log entry — basic unit of Splunk data |
| Index | Storage container for specific data type |
| Field | Key-value pair extracted from an event |
| SPL | Splunk Processing Language — query language |
| `index=` | Specify which data to search |
| `stats count by` | Count events grouped by a field |
| `stats sum() by` | Total a numeric field grouped by another field |
| `sort -field` | Sort descending — highest first |
| `table` | Show only specific fields |
| `where` | Filter results after a pipe |
| `earliest=-24h` | Search last 24 hours |
| `earliest=-1h` | Search last 1 hour |
| EventCode 4625 | Windows failed login |
| EventCode 4624 | Windows successful login |
| Dashboard | Visual real-time display of security metrics |
| Alert | Automated notification when search conditions met |
| Pipe `\|` | Pass results of one command to the next |

---

## 🔍 Cheat Sheet — Daily SOC Searches

```splunk
# All activity from suspicious IP
index=* src_ip=SUSPICIOUS_IP earliest=-24h

# Failed logins — brute force detection
index=windows EventCode=4625 | stats count by src_ip | where count > 10 | sort -count

# Successful logins after hours
index=windows EventCode=4624 | eval hour=strftime(_time,"%H") | where hour < 6 OR hour > 22

# Large outbound transfers — exfiltration detection
index=network | where bytes_out > 100000000 | stats sum(bytes_out) by dest_ip | sort -bytes_out

# Find targeted usernames in brute force
index=main failed earliest=-24h | stats count by user | sort -count
```

---

*Part of the [SOC / Blue Team](./README.md) section of my cybersecurity learning journey.*
