# 🔵 SOC / Blue Team

> This folder documents my complete SOC (Security Operations Centre) and Blue Team learning journey. I have chosen SOC as my primary cybersecurity domain as it is the most in-demand entry-level cybersecurity role in the industry. This section covers theory, hands-on labs, tools, and real-world SOC workflows.

---

## 🎯 Why SOC / Blue Team?

SOC Analyst is the most hired entry-level cybersecurity role — especially in India where companies like TCS, Wipro, Infosys, Deloitte, PwC, and IBM actively hire freshers for L1 SOC positions. Understanding how to defend, monitor, and respond to attacks is also the fastest path to understanding offensive security later.

---

## 📂 Files in This Folder

| File | What it covers | Status |
|---|---|---|
| [`SOC_Fundamentals.md`](./SOC_Fundamentals.md) | What is a SOC, analyst levels, roles, workflow | ✅ Complete |
| [`SIEM_Fundamentals.md`](./SIEM_Fundamentals.md) | What is SIEM, how it works, log sources, use cases | ✅ Complete |
| [`What_is_Splunk.md`](./What_is_Splunk.md) | Splunk architecture, components, web interface, SOC use cases | ✅ Complete |
| [`Splunk_Basics.md`](./Splunk_Basics.md) | Splunk queries, dashboards, searching | ✅ Complete |
| [`Phishing_Analysis.md`](./Phishing_Analysis.md) | Email header analysis, phishing indicators | ✅ Complete |
| [`Threat_Intelligence.md`](./Threat_Intelligence.md) | IOCs, AbuseIPDB, Cisco Talos, threat feeds | 🔄 In Progress |
| [`Alert_Triage.md`](./Alert_Triage.md) | Triage process, false positives, escalation | 🔒 Coming Soon |
| [`Incident_Response.md`](./Incident_Response.md) | IR lifecycle, containment, eradication, recovery | 🔒 Coming Soon |
| [`Log_Analysis.md`](./Log_Analysis.md) | Windows logs, Linux logs, network logs | 🔒 Coming Soon |
| [`Questions_on_SOC.md`](./Questions_on_SOC.md) | Interview Q&A revision sheet | 🔒 Coming Soon |

---

## 🛠️ Practical Labs

| Lab | Platform | Topic | Status |
|---|---|---|---|
| Junior Security Analyst Intro | TryHackMe | SOC workflow simulation, IP triage | ✅ Complete |
| SOC Role in Blue Team | TryHackMe | SOC roles, career path, team structure | ✅ Complete |
| Intro to SIEM | TryHackMe & Splunk official | SIEM concepts, log sources, use cases | ✅ Complete |
| Splunk Basics | TryHackMe & Splunk official | Splunk queries, dashboards, searching | ✅ Complete |
| Investigating with Splunk | TryHackMe | Real investigation using Splunk | ⏳ Upcoming |
| Phishing Analysis Fundamentals | TryHackMe | Phishing email analysis | ⏳ Upcoming |
| Blue Team Labs Online | BTLO | Hands-on defensive challenges | ⏳ Upcoming |

---

## 🔵 SOC Analyst Career Path

```
L1 Analyst (Triage)        ← Target entry-level role
        ↓
L2 Analyst (Incident Response)
        ↓
L3 / Threat Hunter
        ↓
Team Lead
        ↓
SOC Manager
        ↓
CISO (Chief Information Security Officer)
```

**L1 SOC Analyst responsibilities:**
- Monitor and triage security alerts from SIEM
- Investigate suspicious IPs, domains, and files
- Perform threat intelligence lookups
- Escalate confirmed incidents to L2
- Document findings and write incident reports

---

## 🔧 Tools Being Learned

| Tool | Purpose | Status |
|---|---|---|
| **Splunk** | SIEM — search, analyse, and visualise logs | 🔄 Learning |
| **AbuseIPDB** | Check if an IP has been reported as malicious | ✅ Used in lab |
| **Cisco Talos** | IP and domain reputation intelligence | ✅ Used in lab |
| **Wireshark** | Network traffic analysis | ✅ Complete |
| **IBM QRadar** | Enterprise SIEM platform | ⏳ Upcoming |
| **Elastic SIEM** | Open source SIEM | ⏳ Upcoming |

---

## ⚔️ Attacks Studied From a SOC Perspective

| Attack | How SOC Detects It |
|---|---|
| Phishing + C2 | Unusual outbound connections, suspicious email headers |
| SYN Flood | High volume of SYN packets with no ACK in network logs |
| DNS Tunneling | Unusually long DNS queries, high DNS query volume |
| Brute Force | Multiple failed login attempts in authentication logs |
| Lateral Movement | Internal network connections between unusual hosts |
| Data Exfiltration | Large outbound data transfers to unknown IPs |

---

## 📚 Resources Being Used

| Resource | Link | Used For |
|---|---|---|
| TryHackMe SOC Level 1 | [tryhackme.com](https://tryhackme.com) | Guided SOC learning path |
| Blue Team Labs Online | [blueteamlabs.online](https://blueteamlabs.online) | Hands-on defensive labs |
| Splunk Free Training | [splunk.com/en_us/training.html](https://www.splunk.com/en_us/training.html) | SIEM hands-on |
| AbuseIPDB | [abuseipdb.com](https://abuseipdb.com) | Threat intelligence |
| Cisco Talos | [talosintelligence.com](https://talosintelligence.com) | Threat intelligence |

---

*Part of my [Cybersecurity Roadmap](../README.md)*
