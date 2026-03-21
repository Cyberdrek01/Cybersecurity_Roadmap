# 🔵 SOC Fundamentals

> A SOC (Security Operations Centre) is the nerve centre of an organisation's cybersecurity defence. This file covers what a SOC is, how it operates, the roles within it, and the day-to-day workflow of a SOC analyst — the role I am actively working towards.

---

## 📋 Table of Contents
- [What is a SOC?](#what-is-a-soc)
- [Why SOC Exists](#why-soc-exists)
- [SOC Analyst Levels](#soc-analyst-levels)
- [Other SOC Roles](#other-soc-roles)
- [SOC Career Path](#soc-career-path)
- [Day in the Life of an L1 Analyst](#day-in-the-life-of-an-l1-analyst)
- [SOC Triage Workflow](#soc-triage-workflow)
- [Reactive vs Proactive Security](#reactive-vs-proactive-security)
- [Key SOC Concepts](#key-soc-concepts)
- [Real World SOC Lab — TryHackMe Simulation](#real-world-soc-lab--tryhackme-simulation)
- [Quick Reference](#quick-reference)

---

## What is a SOC?

A **SOC (Security Operations Centre)** is a dedicated team and facility responsible for continuously monitoring, detecting, analysing, and responding to cybersecurity threats and incidents within an organisation.

Think of a SOC as a 24/7 security control room — analysts monitor dashboards, investigate alerts, and respond to threats in real time.

**Key responsibilities of a SOC:**
- Monitor networks, systems, and applications for suspicious activity
- Detect and investigate security alerts
- Respond to and contain security incidents
- Perform threat intelligence analysis
- Maintain security tools and infrastructure
- Document incidents and write reports

---

## Why SOC Exists

No organisation can be 100% secure. Attackers are constantly probing for weaknesses. A SOC exists because:

- **Attacks are inevitable** — the question is not if but when
- **Speed matters** — the faster an attack is detected, the less damage it causes
- **Complexity** — modern IT environments generate millions of events per day — humans need tools and processes to find the real threats
- **Compliance** — many industries legally require 24/7 security monitoring

**The SOC's goal** is to minimise the time between an attack starting and it being detected and contained — called **Mean Time to Detect (MTTD)** and **Mean Time to Respond (MTTR)**.

---

## SOC Analyst Levels

### L1 — Triage Analyst
- **This is the entry-level target role**
- First line of defence — monitors the SIEM dashboard
- Reviews and triages incoming security alerts
- Performs initial investigation — checks IPs, domains, file hashes
- Determines if an alert is a true positive or false positive
- Escalates confirmed incidents to L2
- Documents findings

**Skills needed:** SIEM basics, threat intelligence tools, networking fundamentals, alert triage

---

### L2 — Incident Responder
- Receives escalated incidents from L1
- Performs deeper investigation and analysis
- Develops response and containment strategies
- Coordinates with system owners to contain the threat
- Analyses malware and attack techniques

**Skills needed:** Incident response, malware analysis, forensics, advanced SIEM

---

### L3 — Threat Hunter / Senior Analyst
- Proactively hunts for threats that bypassed automated detection
- Does not wait for alerts — actively searches for signs of compromise
- Develops new detection rules and SIEM use cases
- Analyses advanced persistent threats (APTs)

**Skills needed:** Threat hunting, advanced forensics, MITRE ATT&CK framework, scripting

---

## Other SOC Roles

| Role | Responsibility |
|---|---|
| **SOC Engineer** | Builds, maintains, and tunes security tools and SIEM |
| **Team Lead** | Manages L1/L2 analysts, oversees day-to-day operations |
| **SOC Manager** | Strategic oversight of the entire SOC |
| **CISO** | Chief Information Security Officer — executive responsible for all security |

---

## SOC Career Path

```
L1 Analyst (Triage)        ← Entry level — target role
        ↓  1–2 years
L2 Analyst (Incident Response)
        ↓  2–3 years
L3 / Threat Hunter
        ↓
Team Lead / SOC Engineer
        ↓
SOC Manager
        ↓
CISO
```

**The hidden path:** Many pentesters start in SOC. Understanding how attacks are detected makes you a better attacker. SOC → Pentesting is a well-known career transition.

---

## Day in the Life of an L1 Analyst

A typical day for an L1 SOC analyst:

```
Start of shift:
→ Review overnight alerts and incidents
→ Check dashboards for active threats
→ Handover from previous shift analyst

During shift:
→ Monitor SIEM for new alerts
→ Triage alerts — investigate suspicious activity
→ Look up IPs/domains on threat intelligence platforms
→ Determine true positive or false positive
→ Escalate confirmed threats to L2
→ Document all findings

End of shift:
→ Write shift report
→ Handover to next shift analyst
→ Update open incident tickets
```

---

## SOC Triage Workflow

When an alert arrives, an L1 analyst follows this process:

```
Alert triggered in SIEM
        ↓
Step 1: Review the alert
        What triggered it? What asset is affected?
        ↓
Step 2: Gather context
        Is this normal behaviour for this user/system?
        ↓
Step 3: Threat intelligence lookup
        Check the IP/domain/hash on AbuseIPDB, Cisco Talos, VirusTotal
        ↓
Step 4: Determine verdict
        True Positive → real threat, proceed to response
        False Positive → not a threat, document and close
        ↓
Step 5: Response (if True Positive)
        Contain → block IP, isolate system, disable account
        ↓
Step 6: Escalate to L2
        Pass the incident with full documentation
        ↓
Step 7: Document
        Write up findings, timeline, actions taken
```

---

## Reactive vs Proactive Security

| Type | Definition | Examples |
|---|---|---|
| **Reactive** | Responding to threats after they are detected | Blocking a malicious IP, containing malware, incident response |
| **Proactive** | Preventing threats before they cause damage | Threat hunting, patching vulnerabilities, security audits, red team exercises |

**L1/L2 analysts are primarily reactive** — they respond to alerts.
**L3 Threat Hunters are proactive** — they search for threats before alerts fire.

A mature SOC does both.

---

## Key SOC Concepts

### True Positive vs False Positive

| Term | Meaning | Action |
|---|---|---|
| **True Positive** | Alert is real — actual threat detected | Investigate and escalate |
| **False Positive** | Alert is not real — normal activity flagged incorrectly | Document and close |
| **True Negative** | No alert — no threat present | Normal |
| **False Negative** | No alert — but a real threat exists (missed) | Most dangerous — goes undetected |

**False positives** are a major challenge in SOC work — too many false positives cause **alert fatigue** where analysts start ignoring alerts, potentially missing real threats.

### Alert Fatigue
When analysts are overwhelmed by too many alerts — especially false positives — they become desensitised and may miss genuine threats. SOC engineers tune SIEM rules to reduce false positives and improve alert quality.

### IOC — Indicator of Compromise
Evidence that a system may have been compromised:

| IOC Type | Example |
|---|---|
| Malicious IP address | 192.168.1.100 connecting to known C2 server |
| Malicious domain | evil-malware.com |
| File hash (MD5/SHA256) | Hash matching known malware |
| Unusual process | cmd.exe spawned by a Word document |
| Registry modification | Persistence mechanism added |

### Escalation
The process of passing an incident from L1 to L2 when it is confirmed as a true positive or requires deeper investigation. L1 provides full documentation so L2 can continue without starting over.

---

## Real World SOC Lab — TryHackMe Simulation

**Platform:** TryHackMe — Junior Security Analyst Intro

**What was simulated:**
A real SOC alert appeared in the dashboard. An IP address had been flagged as suspicious.

**Steps performed:**

```
Step 1: Alert received — suspicious IP flagged in the system

Step 2: Threat intelligence lookup
        → Checked IP on AbuseIPDB
        → Checked IP on Cisco Talos
        → IP confirmed as malicious — high abuse confidence score

Step 3: Verdict — True Positive
        The IP was genuinely malicious

Step 4: Reported to SOC Team Lead
        → Escalated with full findings

Step 5: Team Lead instructed: Block the IP

Step 6: IP blocked
        → Threat contained

Step 7: Incident documented
```

**Professional terms for each step:**

| Action Taken | Professional Term |
|---|---|
| Received the alert | Alert Triage |
| Checked IP on AbuseIPDB/Talos | Threat Intelligence Lookup |
| Confirmed it was malicious | True Positive Confirmation |
| Reported to Team Lead | Escalation |
| Blocked the IP | Incident Response / Containment |

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| SOC stands for | Security Operations Centre |
| L1 role | Triage — monitor alerts, initial investigation |
| L2 role | Incident response — deeper investigation |
| L3 role | Threat hunting — proactive search for hidden threats |
| MTTD | Mean Time to Detect — how fast threats are found |
| MTTR | Mean Time to Respond — how fast threats are contained |
| True Positive | Real threat — take action |
| False Positive | Not a threat — document and close |
| False Negative | Real threat missed — most dangerous |
| Alert Fatigue | Analysts overwhelmed by too many alerts |
| IOC | Indicator of Compromise — evidence of a breach |
| Escalation | Passing confirmed incident from L1 to L2 |
| Reactive security | Responding after detection |
| Proactive security | Preventing before detection |
| AbuseIPDB | Threat intel — malicious IP database |
| Cisco Talos | Threat intel — IP and domain reputation |

---

*Part of the [SOC / Blue Team](./README.md) section of my cybersecurity learning journey.*
