# 🛡️ Splunk Fundamentals for SOC Analysts

## 📌 Overview
This repository contains my notes and hands-on learning for Splunk (SIEM) from a SOC (Security Operations Center) perspective.

The goal is to understand how Splunk works, write effective SPL queries, and perform real-world security investigations as an L1 SOC Analyst.

---

# 🏗️ 1. How Splunk Works (Mental Model)

In a real organization, hundreds of systems generate logs every second:

2026-03-23 09:15:33 host=webserver-01 src_ip=192.168.1.100 dest_ip=8.8.8.8 action=blocked port=443 user=john

Splunk processes logs by:
- Treating each log line as an event
- Extracting fields (key=value)
- Storing data in an index
- Making it searchable instantly

You are NOT searching raw text — you are searching structured data.

---

# 🏗️ 2. Default Fields in Every Event

Every Splunk event contains these 5 fields:

| Field      | Meaning            | Example                |
|------------|-------------------|------------------------|
| _time      | Timestamp         | 2026-03-23 09:15:33    |
| host       | Machine name      | webserver-01           |
| source     | Log source file   | /var/log/auth.log      |
| sourcetype | Log format        | linux_secure           |
| index      | Storage location  | main                   |

Custom fields:
- src_ip
- dest_ip
- user
- action
- port

---

# 🏗️ 3. SPL (Splunk Processing Language)

## Basic Search
failed

## Search in Index
index=main failed

## Filter by Field
index=main src_ip=192.168.1.100

## Combine Conditions
index=main src_ip=192.168.1.100 action=blocked

## Time Range
index=main failed earliest=-24h latest=now

---

# 🏗️ 4. The Pipe ( | )

The pipe passes results to another command (like Linux pipes).

Example:
index=main failed | stats count by src_ip

Meaning:
1. Find failed events
2. Count occurrences per IP
3. Display results

---

# 🏗️ 5. Important SPL Commands

## stats
index=main | stats count by src_ip

## table
index=main | table _time src_ip dest_ip action

## sort
index=main | stats count by src_ip | sort -count

## head
index=main | head 10

## where
index=main | stats count by src_ip | where count > 100

## rex
index=main | rex "user=(?P<username>\w+)"

---

# 🏗️ 6. Real SOC Use Cases

## Brute Force Detection
index=windows EventCode=4625
| stats count by src_ip
| where count > 10
| sort -count

## Investigate Suspicious IP
index=* src_ip=192.168.1.100
| table _time host action dest_ip dest_port
| sort _time

## After-Hours Login Detection
index=windows EventCode=4624
| eval hour=strftime(_time, "%H")
| where hour < 6 OR hour > 22
| table _time user host

## Data Exfiltration Detection
index=network
| where bytes_out > 100000000
| table _time src_ip dest_ip bytes_out
| sort -bytes_out

---

# 🚨 SOC Investigation Scenario

## Task 1 — All Activity from Suspicious IP
index=main src_ip=203.0.113.55 earliest=-24h latest=now

## Task 2 — Failed Logins by Username
index=main src_ip=203.0.113.55 action=failure earliest=-24h latest=now
| stats count by username
| sort -count

## Task 3 — Successful Login Check
index=main src_ip=203.0.113.55 action=success earliest=-24h latest=now
| table _time user host

---

# 🧠 Incident Analysis

Scenario:
- 847 failed logins
- 1 successful login

Conclusion:
Brute force attack succeeded

L1 SOC Response:
1. Escalate to L2 / Team Lead
2. Block attacker IP
3. Investigate post-login activity:
   - File access
   - Privilege escalation
   - New accounts
   - Outbound traffic
4. Document incident timeline

---

# 🏗️ 7. Dashboards

Dashboards are visual representations of searches.

Common panels:
- Total alerts
- Top attacking IPs
- Failed logins over time

Creation steps:
1. Run search
2. Click Save As → Dashboard Panel
3. Choose visualization

---

# 🚨 Alerts

Alerts are automated searches.

Example:
index=windows EventCode=4625
| stats count by src_ip
| where count > 10

Configuration:
- Schedule: Every 5 minutes
- Trigger: Results > 0
- Action: Email or ticket

---

# 🎯 Data Exfiltration Scenario

## Outbound Connections
index=network src_ip=192.168.1.55 earliest=-1h latest=now
| table _time dest_ip bytes_out

## Bytes Transferred by Destination
index=network src_ip=192.168.1.55 earliest=-1h latest=now
| stats sum(bytes_out) by dest_ip
| sort -sum(bytes_out)

---

## Analysis

Finding:
2GB sent to external IP at 3 AM

Attack Type:
Data Exfiltration

L1 Response:
1. Escalate immediately
2. Block destination IP
3. Investigate:
   - What data was sent
   - Which user initiated it
   - Any malware involved

---

# 🚀 Skills Gained

- Understanding Splunk architecture
- Writing SPL queries
- Log analysis
- SOC investigation workflow
- Incident response basics

---
