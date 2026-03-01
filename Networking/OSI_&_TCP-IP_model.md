# OSI & TCP/IP Model

## OSI- Open System Interconnection 

### The 7 Layers (Top → Bottom)
```bash
  7️⃣ Application
  6️⃣ Presentation
  5️⃣ Session
  4️⃣ Transport
  3️⃣ Network
  2️⃣ Data Link
  1️⃣ Physical
```

#### 1. Application Layer

This is the top most layer in the OSI model.
It is what the user sees.
Here various protocols work like-

* HTTP/HTTPS- Hyper Text Transfer Protocol S for secure http lacks security as all data transmits in plain text ao https should be used for better security. HTTP+SSL/TLS.
* FTP- File Transfer Protocol, it is insecure, unencrypted, legacy protocol data and password in plain text, making it vulnerable to brute force or Main in The Middle attacks. Organization should use SFTP for more security.
* DNS- Domain Name System, Convert the domain names into IP addresses.
* SMTP- Simple Mail Transfer Protocol, used to send mails but it lacks security so SMTP+TLS for encryption.
* SSH- Secure Shell, enables secure,encrypted communication over a network between two devices.
  
This is where web attacks happen-

* SQL injection- It is a web security vulnerability that allows attacker to interfere with the queries an application makes it to the database. By inserting various malicious input fields.
* XSS- Cross-Site Scripting (XSS) is a web security vulnerability where attackers inject malicious client-side scripts (usually JavaScript) into trusted websites
* File upload attacks- Occurs when an web application allows user to upload file into its filesystem. Attackers use this exploit to upload various script which gets executed into the server.

SOC Perspective:
If attacker injects malicious payload in HTTP request → Layer 7 problem.

Logs you’ll see:
  Web server logs
  WAF logs

### 2. Presentation Layer

This handles:
Encryption (SSL/TLS)
Compression
Encoding

SSL (Secure Sockets Layer) and TLS (Transport Layer Security) are cryptographic protocols that secure internet communication, ensuring privacy and data integrity between web browsers and servers. TLS is the modern, more secure successor to SSL, which is now deprecated, though "SSL" is still commonly used to refer to both

SOC Perspective:
If certificate invalid → Layer 6 issue
If HTTPS traffic encrypted → Harder to inspect

Very important:
TLS handshake happens here conceptually. (TLS handshake it is the initial, foundational step establishing connection between the client and the server.)

### 3. Session Layer

Responsible for:
  Session establishment
  Session maintenance
  Session termination

SOC Perspective:
Session hijacking attacks occur here.
(Session hijacking is a cyberattack where an attacker steals or manipulates a user's active session token (cookie) to gain unauthorized access to a web application, effectively impersonating the user without needing login credentials)

In real life:
This layer is often merged with Application in TCP/IP.

### 4.Transport Layer (Critical Layer for SOC)

Protocols:
  TCP- Transmission Control Protocol -It is a conncetion oriented protocol- Prioritize error free, relaible
  
  UDP- User Datagram Protocol - It is connectionless protocol- Priotize speed

Handles:
  Port numbers- 16 bit numbers, that redirect data to specific application or services on a device
  
  Reliability- The probability that a network will perform its required functions without failure over a specific time.
  
  Flow control- A critical mechanism that regulates the rate of data transmission between a fast sender and a slower receiver, preventing the     receiver's buffer from overflowing and causing packet loss
  
  Handshakes- Establishing a synchronised connection

This is extremely important.

SOC must understand:
  
  SYN
  
  ACK
  
  FIN
  
  RST

Attacks here:
 
  SYN Flood: A SYN flood is a type of Denial-of-Service (DoS) attack that exploits the TCP three-way handshake to exhaust server resources and make them unavailable to legitimate users.
  
  Port scanning: Port scanning is a reconnaissance technique used in cybersecurity to probe a server or host for open ports, identifying active services and potential vulnerabilities
  
  Brute force attempts: A brute force attack is a trial-and-error method used by hackers to guess login information, encryption keys, or hidden web pages by systematically trying every possible combination of characters until the correct one is found. 

Logs here:
  
  Firewall logs
  
  IDS alerts

### 5. Network layer

Responsible for:

IP addresses

Routing

Packet forwarding

Attacks:

IP spoofing-  A cyberattack technique where hackers forge the source IP address in network packets to disguise their identity or impersonate another, trusted computer system

Route hijacking- Cyberattack where an attacker illegitimately takes control of groups of IP addresses by corrupting the Internet's routing tables. This is done by manipulating the Border Gateway Protocol (BGP).

Logs:

Router logs

Firewall logs

If wrong IP → Layer 3 issue.

### 6.Data Link Layer (MAC address)

Responsible for:

MAC addresses

ARP

Switching

Attacks:

ARP poisoning: ARP poisoning (or spoofing) is a type of local area network (LAN) cyberattack where an attacker sends fake ARP messages to map their own MAC address to a legitimate user's IP address

MAC flooding: MAC flooding is a network security attack where an attacker overwhelms a switch's MAC address table with thousands of fake MAC addresses.

This is internal network attack zone.

### 7. Physical Layer

Bits over:

Cables

WiFi signals

Hardware

SOC usually doesn’t deal much here unless physical tampering.


### Scenario analysis:

Scenario 1:
Attacker floods a server with SYN packets but never completes handshake.

Which layer?
What attack?
What would logs show?

```bash
✅ Scenario 1

Attacker floods server with SYN packets but never completes handshake

Your answer:

Transport layer ✅

SYN attack ✅

Firewall logs & IDS ✅

Perfect.

This is a SYN Flood (TCP half-open attack)
Layer → 4 (Transport)
What logs show:

Many SYN

No ACK completion

High number of half-open connections

Possibly source IP spoofed

This is exactly how a SOC analyst should think.
```

Scenario 2:
User reports:
“I got redirected to a fake banking website.”

Which layers could be involved?
```bash
Let’s analyze properly.

Redirection can happen at multiple layers:

1️⃣ DNS Spoofing → Layer 7 (Application protocol DNS)
2️⃣ ARP poisoning → Layer 2 (internal network MITM)
3️⃣ HTTP redirect injection → Layer 7
4️⃣ Compromised router DNS settings → Layer 3

SOC thinking means:
Don’t assume one layer.
Think of possible attack vectors.

Most common:
👉 DNS spoofing (Application layer protocol)
👉 Or ARP poisoning (Data Link layer)

So answer should be:
“Could be DNS (Layer 7) or ARP poisoning (Layer 2).”
```

Scenario 3:
Firewall log shows:
192.168.1.5 → 8.8.8.8:53 large repeated DNS queries

Which layer?
What could be happening?
```bash
Port 53 = DNS
DNS belongs to → Application layer (Layer 7 protocol)

Now repeated DNS queries from internal host to 8.8.8.8:

Possibilities:

Normal DNS resolution? Maybe.

Malware beaconing?

DNS tunneling?

Data exfiltration?

Botnet communication?

A DoS attack would target a victim.

But here:
Internal host repeatedly contacting DNS server.

SOC suspicion:
👉 Possible DNS tunneling or malware C2 communication.

Correct layer:
Application layer (DNS)
Transport used could be UDP, but protocol focus is DNS.

SOC thinking:
Look at domain names.
Are they long?
Random characters?
High frequency?
```
Scenario 4:
Firewall log shows:

Source: 185.22.33.9
Destination: 192.168.1.10
Port: 3389
Action: ALLOW
Repeated 200 failed login attempts

What is happening?
Which layer?
What attack?
What should SOC do?

```bash
🔍 Scenario Breakdown

Log:

Source: 185.22.33.9
Destination: 192.168.1.10
Port: 3389
Action: ALLOW
200 failed login attempts
```
```bash
This is most likely:

👉 RDP brute-force attack
Port 3389 = RDP (Remote Desktop Protocol)

Attack type:

Credential brute force

Password spraying

This appears to be an RDP brute-force attack targeting port 3389 over TCP. The repeated failed authentication attempts indicate credential abuse activity at the application layer.
Proper steps:

1️⃣ Confirm attack pattern

Are attempts from single IP?

Or multiple IPs (botnet)?

2️⃣ Check:

Was login successful?

Any account locked?

Any unusual login time?

3️⃣ Containment:

Block IP temporarily (if policy allows)

Add firewall rule

Enable account lockout policy

4️⃣ Escalation:

Inform L2 / IR team if needed

5️⃣ Recommend:

Disable public RDP

Use VPN + MFA

That’s SOC maturity.

⚠ Important Correction

You immediately said:

I would block the IP

In real SOC, we don’t immediately block unless policy allows.

We:

Verify

Confirm

Then act

SOC = Evidence-based action.
```

Scenario 5:

Firewall shows:

Multiple different external IPs
All targeting 192.168.1.15
Ports scanned: 21, 22, 23, 80, 443, 445, 3389
Within 2 minutes

What is happening?
Which layer is being abused?
What type of attack?
What is the attacker trying to find?
```bash
✅ What Is This?

This is:

👉 Distributed Port Scanning
👉 Reconnaissance Activity

The attacker is scanning for open services.

If multiple IPs → Possibly:

Botnet-based scanning

Mass internet scan

🧠 Which Layer Is Being Abused?

Primary Layer:
👉 Transport Layer (Layer 4)

Because port scanning = probing TCP/UDP ports.

Secondarily:
👉 Network Layer (Layer 3) — IP targeting.

Application layer is NOT yet attacked.
They’re just checking what services are open.

🎯 What Is Attacker Trying To Find?

They are checking:

21 → FTP

22 → SSH

23 → Telnet

80 → HTTP

443 → HTTPS

445 → SMB

3389 → RDP

These are common vulnerable services.

Attacker goal:

Identify exposed services

Find weak entry point

Plan next exploitation step

This is Phase 1 of an attack:
Recon → Enumeration → Exploitation

🛡 SOC Thinking

If you see this in logs:

You should:

1️⃣ Check if ports are open
2️⃣ Check if scanning succeeded
3️⃣ Check for follow-up login attempts
4️⃣ Check threat intel on source IPs
5️⃣ Determine if internet-facing system is hardened

Blocking may not always be needed.
Internet scanning is constant.

SOC must differentiate:
Background internet noise vs targeted attack.
```

Scenario 6:

Internal host 192.168.1.25
Starts sending small encrypted HTTPS requests
Every 60 seconds
To an unknown external IP
Even at 3 AM

No user logged in.

What does this indicate?
Which layer?
What is suspicious?

```bash
🚨 What This Likely Is

This strongly indicates:

👉 Command and Control (C2) beaconing
👉 Malware callback communication

Malware often:

“Phones home”

Sends small heartbeat signals

Uses HTTPS to hide inside normal traffic

Communicates at fixed intervals

That 60-second repetition is the biggest red flag.

🌐 Which Layer?

Primary protocol:
HTTPS → Application Layer (Layer 7)

Transport:
TCP → Layer 4

Network:
IP → Layer 3

But SOC answer:

This is suspicious Layer 7 HTTPS traffic showing beaconing behavior.

🔥 Why Is It Suspicious?

1️⃣ Regular interval (exact timing = automation)
2️⃣ Small packets (heartbeat)
3️⃣ No user activity
4️⃣ Unknown external IP

Humans are irregular.
Malware is precise.

🛡 What SOC Should Do

1️⃣ Check destination IP reputation
2️⃣ Look at domain (if any)
3️⃣ Inspect SSL certificate
4️⃣ Check process generating traffic (on endpoint)
5️⃣ Correlate with EDR logs
6️⃣ Isolate host if confirmed malicious

This is how SOC catches malware.
```
🧠 SOC-Level Insight

Modern malware:

Uses HTTPS

Uses cloud services (AWS, Azure, Google Cloud)

Uses domain fronting

Mimics browser user-agent strings

So basic firewall monitoring is not enough.

SOC must detect:

Unusual frequency

Beacon timing patterns

Rare domains

Newly registered domains

IP reputation

No user activity correlation


Internal host makes 500 DNS queries in 3 minutes.
The domain names look like:

asjdhqwe123.example.com
zxqplmn098.example.com
qwertyu567.example.com

Very long and random subdomains.

What does this indicate?

```bash
Malicious, automated attempt at DNS Tunneling, Command-and-Control (C2) communication, or a Random Subdomain (Water Torture) DDoS attack. 

The high volume of queries (500 in 3 minutes) combined with long, random, and likely non-existent (NXDOMAIN) subdomains is a hallmark of malware behavior trying to evade detection. 
```
