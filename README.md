# 🛡️ Cybersecurity Roadmap

> This repository documents my complete cybersecurity learning journey from absolute beginner to job-ready. It is updated daily as I learn new topics, complete labs, solve CTF challenges, and build hands-on projects.

---

## 👤 About This Repository

I am a B.Tech Information Technology student actively working towards breaking into the cybersecurity field. This repository is my public learning journal — every topic I study, every concept I revise, every challenge I solve, and every tool I learn gets documented here.

It serves three purposes:
- **Personal revision** — structured notes I can come back to anytime
- **Portfolio** — proof of consistent learning for potential employers
- **Community** — anyone else learning cybersecurity can use these notes freely

This repository is also my way of keeping myself accountable and motivated to learn consistently every single day. Making my progress public pushes me to show up daily, document what I learn, and keep moving forward — no matter how small the progress is on a given day.

**Current Status:** 🟢 Actively updated — new content added regularly

> **📝 A note on content:** Some notes and explanations in this repository are AI-assisted — used as a tool to structure, format, and improve the clarity of what I have personally learned. All concepts have been studied, understood, and verified by me. The learning is real — AI just helps me document it better.

---

## ⚠️ Repository Update Policy

> This repository is **updated regularly and consistently.**
> I am currently learning new cybersecurity topics daily and documenting them here as I go.
> Each section will grow over time — theory notes come first, followed by practical labs, tool usage, and real-world writeups.
> Check back often — new folders and files are added as topics are completed.

---

## 📂 Repository Structure

```
cybersecurity_roadmap/
│
├── 📁 networking/               ✅ Theory Complete | ✅ Practicals Complete
│   ├── README.md
│   ├── OSI_model.md
│   ├── Switches.md
│   ├── Routers.md
│   ├── DNS.md
│   ├── Firewall.md
│   ├── HTTP_or_HTTPS.md
│   ├── TCP_VS_UDP.md
│   ├── Questions_on_networking.md
│   └── practicals/
│       └── wireshark/
│           ├── README.md
│           ├── Lab_01_Live_Traffic_Capture.md
│           ├── Lab_02_DNS_Traffic_Analysis.md
│           ├── Lab_03_TCP_Handshake.md
│           └── Lab_04_HTTP_Stream.md
│
├── 📁 linux/                    🔄 In Progress
│   └── bandit-writeups/         (OverTheWire Bandit — 15/34 levels complete)
│
├── 📁 soc-blue-team/            🔄 In Progress — Domain Selected
│   └── README.md
│
├── 📁 tools/                    🔒 Coming Soon
├── 📁 ctf-writeups/             🔒 Coming Soon
└── 📁 interview-prep/           🔒 Coming Soon
```

---

## 📚 Content Sections

### 📡 Networking — Complete
> Status: ✅ Theory Complete | ✅ Practicals Complete

The networking section covers all foundational networking concepts required for cybersecurity. Each file contains theory, step-by-step breakdowns, security relevance, attack explanations, and defences.

Topics covered:
- OSI Model — all 7 layers, protocols, attacks per layer
- Switches — MAC tables, MAC Flooding, ARP Poisoning
- Routers — routing tables, NAT, IP addressing
- DNS — resolution chain, record types, DNS attacks
- Firewalls — types, stateful vs stateless, firewall bypass techniques
- HTTP & HTTPS — request/response cycle, cookies, sessions, web attacks
- TCP vs UDP — Three-Way Handshake, TCP flags, SYN Flood attack
- Q&A Revision Sheet — 42 interview-style questions with model answers

Practicals completed:
- Wireshark Lab 01 — Live traffic capture on eth0
- Wireshark Lab 02 — DNS query and response analysis
- Wireshark Lab 03 — TCP Three-Way Handshake capture
- Wireshark Lab 04 — HTTP stream interception using curl

📂 [Go to Networking →](./Networking/)

---

### 🐧 Linux — In Progress
> Status: 🔄 In Progress | OverTheWire Bandit: 15/34 levels complete

Hands-on Linux skills through OverTheWire Bandit wargames. Each level teaches real Linux and cybersecurity concepts — file permissions, encoding/decoding, SSH keys, port scanning, privilege escalation, and more.

📂 [Go to Linux →](./linux/) *(coming soon)*

---

### 🔵 SOC / Blue Team — In Progress
> Status: 🔄 In Progress | Domain selected — actively learning

Selected **SOC / Blue Team** as my primary cybersecurity domain. This is the most in-demand entry-level cybersecurity role in the industry with the highest volume of fresher job openings.

Topics being covered:
- SOC roles and responsibilities — L1, L2, L3, Threat Hunter
- SIEM tools — Splunk, IBM QRadar
- Alert triage and incident response workflow
- Log analysis and threat intelligence
- Phishing email analysis
- Network traffic analysis for SOC
- Blue Team Labs Online challenges

📂 [Go to SOC / Blue Team →](./soc-blue-team/)

---

### 🔧 Tools — Coming Soon
> Status: 🔒 Coming Soon

Will cover hands-on usage of: Splunk, Wireshark, Nmap, Burp Suite, and more.

---

### 🏴 CTF Writeups — Coming Soon
> Status: 🔒 Locked — starting from Week 9

Will document solutions to PicoCTF and Hack The Box challenges with full explanations of the methodology and tools used.

---

### 🎯 Interview Prep — Coming Soon
> Status: 🔒 Locked — starting from Week 13

Will contain common SOC and cybersecurity interview questions with model answers, covering technical concepts, tool knowledge, and scenario-based questions.

---

## 📈 Learning Roadmap & Progress

| Phase | Section | Status | Timeline |
|---|---|---|---|
| Phase 1 | Networking Theory | ✅ Complete | Week 1 |
| Phase 1 | Linux — Bandit Wargames | 🔄 In Progress (15/34) | Week 1–2 |
| Phase 1 | Networking Practicals — Wireshark | ✅ Complete | Week 1 |
| Phase 2 | Domain Selected — SOC / Blue Team | ✅ Complete | Week 2 |
| Phase 2 | SOC / Blue Team — TryHackMe Path | 🔄 In Progress | Week 2–4 |
| Phase 2 | SIEM — Splunk Hands-on | ⏳ Upcoming | Week 3–4 |
| Phase 2 | Blue Team Labs Online | ⏳ Upcoming | Week 4 |
| Phase 3 | CTF Competitions + Home Lab | 🔒 Locked | Week 9–10 |
| Phase 3 | GitHub Portfolio + Certification | 🔒 Locked | Week 11–12 |
| Phase 4 | Job/Internship Applications | 🔒 Locked | Week 13–14 |
| Phase 4 | Interview Preparation | 🔒 Locked | Week 15–16 |

---

## ⚔️ Attacks Learned So Far

| Attack | Category | Topic |
|---|---|---|
| MAC Flooding (CAM Table Overflow) | Layer 2 | Switches |
| MAC Spoofing | Layer 2 | Switches |
| ARP Poisoning | Layer 2/3 | Switches / Routers |
| IP Spoofing | Layer 3 | Routers |
| BGP Hijacking | Layer 3 | Routers |
| DNS Spoofing | Application | DNS |
| DNS Cache Poisoning | Application | DNS |
| DNS Tunneling | Firewall Bypass | DNS |
| DNS Amplification (DDoS) | Application | DNS |
| MITM — Man in the Middle | Application | HTTP |
| Session Hijacking | Application | HTTP |
| SSL Stripping | Application | HTTP/HTTPS |
| Clickjacking | Application | HTTP |
| SYN Flood | Transport | TCP/UDP |
| UDP Flood | Transport | TCP/UDP |
| Phishing + C2 Bypass | Multi-layer | Firewalls |

---

## 🔧 Tools Being Learned

| Tool | Purpose | Status |
|---|---|---|
| **Wireshark** | Packet capture and traffic analysis | ✅ Practicals Complete |
| **Splunk** | SIEM — log analysis and threat detection | 🔄 Learning |
| **AbuseIPDB** | Threat intelligence — malicious IP lookup | 🔄 Learning |
| **Cisco Talos** | Threat intelligence — IP and domain reputation | 🔄 Learning |
| **Nmap** | Port scanning and service discovery | ⏳ Upcoming |
| **Burp Suite** | Web application testing | ⏳ Upcoming |
| **Metasploit** | Exploitation framework | ⏳ Upcoming |

---

## 📚 Resources Being Used

| Resource | Link | Used For |
|---|---|---|
| NetworkChuck | [youtube.com/@NetworkChuck](https://youtube.com/@NetworkChuck) | Networking & general cyber concepts |
| Professor Messer | [professormesser.com](https://professormesser.com) | Networking theory reference |
| TryHackMe | [tryhackme.com](https://tryhackme.com) | Hands-on labs and guided paths |
| OverTheWire | [overthewire.org](https://overthewire.org/wargames) | Linux and networking wargames |
| Blue Team Labs Online | [blueteamlabs.online](https://blueteamlabs.online) | SOC and defensive security labs |
| Splunk Training | [splunk.com/en_us/training.html](https://www.splunk.com/en_us/training.html) | Free SIEM training |
| PicoCTF | [picoctf.org](https://picoctf.org) | CTF competitions (upcoming) |
| Hack The Box | [hackthebox.com](https://hackthebox.com) | Advanced labs (upcoming) |

---

## 🤝 Connect & Feedback

If you are also on a cybersecurity learning journey, feel free to:
- ⭐ Star this repository to follow my progress
- 🐛 Open an issue if you spot an error in the notes
- 💡 Any suggestions, guidance, or advice are always welcome — I am a beginner and would love to hear from people with more experience in the field

📧 **Email:** manasjyotiboro09@gmail.com
💼 **LinkedIn:** [Manas Jyoti Boro](https://www.linkedin.com/in/manas-jyoti-boro/)

> *"Every expert was once a beginner. This repository is proof that I showed up today."*

---

*This repository is actively maintained and updated daily.*
*New topics, practicals, writeups, and tools are added as I progress through my learning journey.*
*Last updated: regularly — check commit history for latest changes*
