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
  SYN Flood
  Port scanning
  Brute force attempts

Logs here:
  Firewall logs
  IDS alerts
