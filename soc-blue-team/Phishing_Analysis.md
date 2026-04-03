# 🎣 Phishing Analysis Fundamentals

> Phishing is the most common initial attack vector in cybersecurity. The majority of security incidents start with a phishing email. As an L1 SOC analyst, analysing suspicious emails is one of the most frequent day-to-day tasks. This file covers how to identify, analyse, and respond to phishing emails.

---

## 📋 Table of Contents
- [What is Phishing?](#what-is-phishing)
- [Types of Phishing](#types-of-phishing)
- [Email Structure](#email-structure)
- [Email Authentication Protocols](#email-authentication-protocols)
- [Phishing Indicators](#phishing-indicators)
- [Email Spoofing](#email-spoofing)
- [Phishing Kits](#phishing-kits)
- [Tools for Phishing Analysis](#tools-for-phishing-analysis)
- [SOC Phishing Analysis Workflow](#soc-phishing-analysis-workflow)
- [Real Investigation Scenario](#real-investigation-scenario)
- [Quick Reference](#quick-reference)

---

## What is Phishing?

**Phishing** is a form of cyberattack in which the attacker pretends to be from a legitimate source to trick the victim into:
- Giving up credentials (username and password)
- Revealing sensitive personal or financial information
- Installing malware or malicious attachments
- Transferring money or gift cards
- Clicking malicious links

Phishing exploits **human psychology** rather than technical vulnerabilities — it is social engineering delivered via email.

---

## Types of Phishing

| Type | Description | Target |
|---|---|---|
| **Phishing** | Mass emails sent to many targets | General public |
| **Spear Phishing** | Highly targeted email tailored to specific individual | Named individual |
| **Whaling** | Spear phishing targeting executives | CEO, CFO, Directors |
| **Smishing** | Phishing via SMS text message | Mobile users |
| **Vishing** | Phishing via voice call | Phone users |
| **Business Email Compromise (BEC)** | Impersonating a company executive to trick employees | Finance teams |

---

## Email Structure

Understanding email structure is essential for phishing analysis. Every email has two parts:

### Email Header
Contains metadata about the email — where it came from, how it travelled, authentication results.

**Key header fields:**

| Field | Purpose | Phishing Relevance |
|---|---|---|
| `From` | Shows who the email appears to be from | Can be faked — check carefully |
| `Reply-To` | Where replies actually go | Attacker sets this to their address |
| `Return-Path` | Where bounced emails go | Often reveals true sender |
| `Received` | List of servers the email passed through | Traces true origin |
| `X-Originating-IP` | IP address that sent the email | Check on AbuseIPDB |
| `Subject` | Email subject line | Urgency/threats are red flags |
| `Date` | When email was sent | Unusual times suspicious |
| `Message-ID` | Unique identifier for the email | Useful for tracking |

### From vs Reply-To — Critical Distinction

```
From:     support@paypal.com        ← looks legitimate
Reply-To: attacker@evil.com         ← where your reply actually goes
```

An attacker makes the **From** field look legitimate but sets **Reply-To** to their own address. When the victim replies, they are communicating directly with the attacker without realising it.

### The Received Chain
The `Received` headers show every server the email passed through — read from bottom to top:

```
Received: from mail.paypal.com      ← 3rd hop (oldest)
Received: from suspicious-server.ru ← 2nd hop
Received: from your-mail-server.com ← 1st hop (most recent)
```

If the email claims to be from PayPal but the original server is `suspicious-server.ru` — it is spoofed.

### Email Body
The actual content of the email — text, HTML, images, links, and attachments.

---

## Email Authentication Protocols

Three protocols verify whether an email is legitimate. Phishing emails frequently fail one or more of these:

### SPF — Sender Policy Framework
Specifies which mail servers are authorised to send email on behalf of a domain.

```
DNS Record: paypal.com SPF → "only these servers can send for us"

Email arrives claiming to be from paypal.com
Server checks: did it come from an authorised server?
→ YES → SPF Pass ✅
→ NO  → SPF Fail ❌ → likely spoofed
```

### DKIM — DomainKeys Identified Mail
Adds a **digital signature** to every email. The receiving server verifies the signature using a public key in DNS.

```
Sender signs email with private key → signature added to header
Receiver verifies signature with public key from DNS
→ Signature valid   → DKIM Pass ✅
→ Signature invalid → DKIM Fail ❌ → email was tampered with
```

### DMARC — Domain-based Message Authentication Reporting and Conformance
Tells receiving servers what to do when SPF or DKIM fails:

| DMARC Policy | Action |
|---|---|
| `none` | Take no action — just report |
| `quarantine` | Send to spam folder |
| `reject` | Block the email entirely |

### What to Look For in Headers
```
Authentication-Results:
  spf=fail       ← SPF failed — not from authorised server
  dkim=fail      ← DKIM failed — email was tampered
  dmarc=fail     ← DMARC failed — domain policy violated
```

All three failing = almost certainly a spoofed phishing email.

---

## Phishing Indicators

### In the Sender Address
- Domain typosquatting — `paypa1.com`, `arnazon.com`, `suppport.com`
- Extra subdomains — `paypal.com.login.evil.com` (real domain is `evil.com`)
- Random character strings — `a7x2k@random-domain.com`
- Free email providers for business — `ceo-company@gmail.com`

### In the Email Content
- **Urgency** — "Your account will be suspended in 24 hours"
- **Threats** — "Failure to respond will result in legal action"
- **Too good to be true** — "You have won a prize"
- **Generic greeting** — "Dear Customer" instead of your name
- **Poor grammar and spelling** — common in mass phishing
- **Requests for sensitive info** — legitimate companies never ask for passwords via email

### In Links
- HTTP instead of HTTPS
- Typosquatted domains — `paypa1.com`
- URL does not match display text — "Click here" links to `evil.com`
- URL shorteners hiding the real destination — `bit.ly/xyz`
- Suspicious paths — `/login`, `/verify`, `/update-account`

### In Attachments
- Unexpected attachments — especially from unknown senders
- Dangerous file types — `.exe`, `.vbs`, `.js`, `.macro-enabled .docx`
- Password protected archives — used to bypass email scanners
- Double extensions — `invoice.pdf.exe`

---

## Email Spoofing

**Email spoofing** is when an attacker fakes the sender address to make an email appear to come from a trusted source.

### How Attackers Spoof Emails
- Modifying the `From` header in the email
- Sending from lookalike domains (`paypa1.com`)
- Exploiting mail servers with weak SPF/DMARC configuration
- Using compromised legitimate email accounts

### Why Spoofing Works
The original email protocol (SMTP) has **no built-in authentication** — anyone can put anything in the From field. SPF, DKIM, and DMARC were created later to address this weakness.

### Spoofing vs Legitimate Email
```
Legitimate email from PayPal:
From: support@paypal.com
SPF: Pass ✅  DKIM: Pass ✅  DMARC: Pass ✅
Received: from mail.paypal.com

Spoofed phishing email:
From: support@paypal.com    ← faked
SPF: Fail ❌  DKIM: Fail ❌
Received: from evil-server.ru  ← true origin
```

---

## Phishing Kits

A **phishing kit** is a ready-made package that allows attackers to launch phishing campaigns quickly — even without technical knowledge.

### What a Phishing Kit Contains
- Fake login pages — clones of real websites (PayPal, Gmail, banks)
- Email templates — pre-written phishing emails
- Backend scripts — capture and store stolen credentials
- Hosting setup instructions — how to deploy the fake site

### Why Phishing Kits are Dangerous
- Lower the barrier to entry — anyone can launch a phishing attack
- Professional looking fake pages that fool even careful users
- Credentials are automatically collected and emailed to attacker
- Kits are sold and shared on dark web forums

---

## Tools for Phishing Analysis

### CyberChef
**Link:** [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef)

A web-based tool for data analysis and decoding. Used in phishing analysis to:
- Decode Base64 encoded email content
- Decode URL encoding
- Extract and analyse email artifacts
- Decode obfuscated malicious scripts

**Example use:**
Attackers often Base64 encode malicious links in emails to bypass filters:
```
aHR0cHM6Ly9ldmlsLmNvbS9sb2dpbg==
```
Paste into CyberChef → From Base64 → reveals: `https://evil.com/login`

---

### VirusTotal
**Link:** [virustotal.com](https://virustotal.com)

Scans files, URLs, domains, and IP addresses against 70+ antivirus engines and threat intelligence feeds.

**Used for:**
- Check if a URL is known malicious
- Check if an attachment hash matches known malware
- Check reputation of a sender IP address
- Analyse suspicious domains

**How to use for phishing:**
1. Copy the suspicious URL from the email
2. Go to VirusTotal → URL tab
3. Paste URL → Scan
4. Check results — any red flags from security vendors = malicious

**Never click suspicious links** — always check on VirusTotal first.

---

## SOC Phishing Analysis Workflow

When an employee forwards a suspicious email, follow this process:

```
Step 1: CHECK EMAIL HEADERS
        → Verify From vs Reply-To — do they match?
        → Check SPF, DKIM, DMARC results
        → Trace Received chain — where did it really originate?
        → Check originating IP on AbuseIPDB

Step 2: ANALYSE SENDER
        → Is the domain legitimate or typosquatted?
        → When was the domain registered? (Whois lookup)
        → Has this sender emailed the organisation before?
        → Check sender domain on VirusTotal

Step 3: INSPECT LINKS
        → Hover over links — does URL match display text?
        → Check URL on VirusTotal
        → Decode any encoded URLs using CyberChef
        → Look for typosquatting, HTTP, suspicious paths

Step 4: ANALYSE ATTACHMENTS
        → NEVER open attachments directly
        → Get the file hash (MD5/SHA256)
        → Check hash on VirusTotal
        → Use a sandbox for dynamic analysis

Step 5: DETERMINE VERDICT
        → Legitimate — no action needed
        → Suspicious — monitor and continue investigating
        → Malicious — proceed to response

Step 6: RESPONSE (if Malicious)
        → Escalate to L2/Team Lead
        → Block sender domain at email gateway
        → Check if other employees received the same email
        → Check if anyone clicked the link or opened attachment
        → Isolate affected endpoints if needed
        → Document full findings and timeline
```

---

## Real Investigation Scenario

**Situation:** An employee forwards a suspicious email claiming to be from PayPal asking them to verify their account urgently.

**Email details:**
```
From:     security@paypal.com
Reply-To: support@paypa1-secure.com
Subject:  URGENT: Your account has been suspended
Link:     http://paypa1-secure.com/verify/login
SPF:      Fail
DKIM:     Fail
DMARC:    Fail
```

**Analysis:**

| Indicator | Finding | Verdict |
|---|---|---|
| From vs Reply-To | Different domains | 🚨 Suspicious |
| SPF/DKIM/DMARC | All failed | 🚨 Spoofed |
| Subject line | Urgency tactic | 🚨 Suspicious |
| Domain | `paypa1` — typosquatted | 🚨 Malicious |
| Protocol | HTTP not HTTPS | 🚨 Suspicious |
| Link path | `/verify/login` | 🚨 Credential harvesting |
| VirusTotal check | Flagged as phishing | 🚨 Confirmed malicious |

**Verdict: MALICIOUS — Phishing attempt**

**Response:**
```
1. Escalate to L2 with full findings
2. Block paypa1-secure.com at email gateway
3. Check mail logs — did other employees receive this?
4. Check proxy logs — did anyone click the link?
5. If clicked → isolate endpoint, reset credentials
6. Document full incident report
```

---

## Quick Reference

| Concept | Key Detail |
|---|---|
| Phishing | Pretending to be legitimate to steal credentials or install malware |
| Spear Phishing | Targeted phishing at a specific individual |
| Whaling | Spear phishing targeting executives |
| Email Spoofing | Faking the sender address |
| From field | Who email appears to be from — can be faked |
| Reply-To field | Where replies actually go — attacker sets this |
| SPF | Verifies sending server is authorised |
| DKIM | Verifies email was not tampered with |
| DMARC | Policy for what to do when SPF/DKIM fails |
| Typosquatting | Fake domain that looks like real one — paypa1.com |
| Phishing kit | Ready-made package for launching phishing attacks |
| CyberChef | Tool for decoding and analysing email artifacts |
| VirusTotal | Scan URLs, files, IPs against 70+ security engines |
| Never click | Always check suspicious links on VirusTotal first |

---

*Part of the [SOC / Blue Team](./README.md) section of my cybersecurity learning journey.*
