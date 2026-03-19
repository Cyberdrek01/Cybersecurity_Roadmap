# 🛡️ Cybersecurity Roadmap

> This repository documents my complete cybersecurity learning journey from absolute beginner to job-ready. It is updated daily as I learn new topics, complete labs, solve CTF challenges, and build hands-on projects.

---

## 👤 About This Repository

I am a third-year B.Tech IT student actively working towards breaking into the cybersecurity field. This repository is my public learning journal — every topic I study, every concept I revise, every challenge I solve, and every tool I learn gets documented here.

It serves three purposes:
- **Personal revision** — structured notes I can come back to anytime
- **Portfolio** — proof of consistent learning for potential employers
- **Community** — anyone else learning cybersecurity can use these notes freely

This repository is also my way of keeping myself accountable and motivated
to learn consistently every single day. Making my progress public pushes me
to show up daily, document what I learn, and keep moving forward — no matter
how small the progress is on a given day.

**Current Status:** 🟢 Actively updated — new content added regularly

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
├── 📁 networking/               ✅ Theory Complete — Practicals Coming Soon
│   ├── README.md
│   ├── OSI_model.md
│   ├── Switches.md
│   ├── Routers.md
│   ├── DNS.md
│   ├── Firewall.md
│   ├── HTTP_or_HTTPS.md
│   ├── TCP_VS_UDP.md
│   └── Questions_on_networking.md
│
├── 📁 linux/                    🔄 In Progress
│   └── bandit-writeups/         (OverTheWire Bandit — 12/34 levels complete)
│
├── 📁 web-security/             🔒 Coming Soon
├── 📁 tools/                    🔒 Coming Soon
├── 📁 ctf-writeups/             🔒 Coming Soon
└── 📁 interview-prep/           🔒 Coming Soon
```

---

## 📚 Content Sections

### 📡 Networking — Theory Complete
> Status: ✅ Theory Complete | 🔄 Practicals Coming Soon

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

📂 [Go to Networking →](./Networking/)

---

### 🐧 Linux — In Progress
> Status: 🔄 In Progress | OverTheWire Bandit: 12/34 levels complete

Hands-on Linux skills through OverTheWire Bandit wargames. Each level teaches real Linux and cybersecurity concepts — file permissions, encoding/decoding, SSH keys, port scanning, privilege escalation, and more.

📂 [Go to Linux →](./linux/) *(coming soon)*

---

### 🌐 Web Security — Coming Soon
> Status: 🔒 Locked — starting after networking practicals

Will cover OWASP Top 10, SQL Injection, XSS, IDOR, CSRF, Burp Suite usage, and PortSwigger Web Security Academy labs.

---

### 🔧 Tools — Coming Soon
> Status: 🔒 Locked

Will cover hands-on usage of: Nmap, Burp Suite, Wireshark, Metasploit, Gobuster, Nikto, and more.

---

### 🏴 CTF Writeups — Coming Soon
> Status: 🔒 Locked — starting from Week 9

Will document solutions to PicoCTF and Hack The Box challenges with full explanations of the methodology and tools used.

---

### 🎯 Interview Prep — Coming Soon
> Status: 🔒 Locked — starting from Week 13

Will contain common cybersecurity interview questions with model answers, covering technical concepts, tool knowledge, and scenario-based questions.

---

## 📈 Learning Roadmap & Progress

| Phase | Section | Status | Timeline |
|---|---|---|---|
| Phase 1 | Networking Theory | ✅ Complete | Week 1–2 |
| Phase 1 | Linux — Bandit Wargames | 🔄 In Progress (12/34) | Week 1–2 |
| Phase 2 | Networking Practicals — Wireshark, Nmap | 🔄 Starting Soon | Week 3–4 |
| Phase 2 | Pick Domain — Pentest / SOC / AppSec | ⏳ Upcoming | Week 5–6 |
| Phase 2 | Domain Deep Dive + Tools | ⏳ Upcoming | Week 7–8 |
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
| **Wireshark** | Packet capture and traffic analysis | 🔄 Learning |
| **Nmap** | Port scanning and service discovery | 🔄 Learning |
| **Kali Linux** | Penetration testing OS | ⏳ Upcoming |
| **Burp Suite** | Web application testing | ⏳ Upcoming |
| **Metasploit** | Exploitation framework | ⏳ Upcoming |
| **Gobuster** | Directory and subdomain enumeration | ⏳ Upcoming |

---

## 📚 Resources Being Used

| Resource | Link | Used For |
|---|---|---|
| NetworkChuck | [youtube.com/@NetworkChuck](https://youtube.com/@NetworkChuck) | Networking & general cyber concepts |
| Professor Messer | [professormesser.com](https://professormesser.com) | Networking theory reference |
| TryHackMe | [tryhackme.com](https://tryhackme.com) | Hands-on labs and guided paths |
| OverTheWire | [overthewire.org](https://overthewire.org/wargames) | Linux and networking wargames |
| PortSwigger | [portswigger.net/web-security](https://portswigger.net/web-security) | Web security labs (upcoming) |
| PicoCTF | [picoctf.org](https://picoctf.org) | CTF competitions (upcoming) |
| Hack The Box | [hackthebox.com](https://hackthebox.com) | Advanced labs (upcoming) |

---

## 🤝 Connect & Feedback

If you are also on a cybersecurity learning journey, feel free to:
- ⭐ Star this repository to follow my progress
- 🐛 Open an issue if you spot an error in the notes
- 💡 Any suggestions, guidance, or advice are always welcome — I am a beginner
  and would love to hear from people with more experience in the field

📧 **Email:** manasjyotiboro09@gmail.com
💼 **LinkedIn:** [Manas Jyoti Boro](https://www.linkedin.com/in/manas-jyoti-boro/)

> *"Every expert was once a beginner. This repository is proof that I showed up today."*
---

*This repository is actively maintained and updated daily.*
*New topics, practicals, writeups, and tools are added as I progress through my learning journey.*
*Last updated: March 2026*
