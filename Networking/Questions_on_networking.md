## Topic 1 — What is a Network?
Q1. What actually is a network?
```
A network is a connection of two or more devices/nodes that communicate and share resources (files, printers, internet) with each other.
```
Q2. What are the two main types of networks and the difference?
```
LAN (Local Area Network) covers a small area like a home or office. WAN (Wide Area Network) covers a large area — the internet itself is the biggest WAN. MAN (Metropolitan Area Network) covers a city.
```
Q3. What is an IP address and why does every device need one?
```
An IP address is a unique identifier assigned to every device on a network. It is needed to identify and communicate with specific devices. There are two types — Public IP (visible on the internet) and Private IP (used inside a LAN, e.g. 192.168.x.x).
```
Q4. Wired vs wireless — which is easier to attack and why?
```
Wireless is easier to attack because the signal travels through the air — an attacker can intercept traffic without any physical access to cables or devices. Wired attacks require physical access to the network infrastructure, making them much harder.
```
Q5. An attacker is "inside the LAN" — what does that mean and why is it dangerous?
```
It means the attacker has gained access to the local network. This is more dangerous than an outside attacker because they can directly sniff traffic of other devices, launch Man-in-the-Middle attacks, and access internal systems that are never exposed to the internet.
```


## Topic 2 — Public vs Private IP
Q1. Difference between Public and Private IP?
```
Private IPs are used inside local networks (e.g. 192.168.x.x) and are not routable on the internet. Public IPs are globally unique addresses used to communicate over the internet. A router uses NAT to convert private IPs to public IPs when going online.
```
Q2. Your laptop has 192.168.1.45 — Public or Private?
```
Private IP. It falls in the 192.168.x.x range which is a reserved private range. The three private IP ranges are: 10.0.0.0–10.255.255.255, 172.16.0.0–172.31.255.255, and 192.168.0.0–192.168.255.255. Any IP outside these ranges is a Public IP.
```
Q3. Two homes both have a device with 192.168.1.1 — how is that possible?
```
Private IP addresses only need to be unique within their own network. Two completely separate networks can reuse the same private IP ranges because they never directly communicate with each other. Conflict only occurs if two devices share the same IP on the same network.
```
Q4. As an attacker, why is knowing someone's Public IP useful?
```
With a target's public IP you can: run an Nmap port scan to discover open services, geolocate the target to identify their city and ISP, launch DoS/DDoS attacks, and attempt to exploit open services like brute forcing SSH on port 22.
```


## Topic 3 — Switch & MAC Addresses
Q1. What is a switch and what is its main job?
```
A switch is a network device that connects multiple devices within a LAN. Its main job is to map MAC addresses to physical ports and forward data only to the correct port — unlike a hub which broadcasts to all devices.
```
Q2. What is a MAC address and how is it different from an IP address?
```
A MAC address is a unique physical identifier burned into a network device's hardware. Format: 00:1A:2B:3C:4D:5E. Unlike an IP address which can change across networks, a MAC address is tied to the hardware — however it can be temporarily changed in software through a technique called MAC Spoofing.
```
Q3. Device A wants to send data to Device C — walk through the process.
```
Device A sends data with Device C's MAC address as the destination. The switch receives it, looks up the destination MAC in its MAC address table, finds which physical port Device C is connected to, and sends the data only to that port. If the MAC is unknown, the switch temporarily broadcasts to all ports until it learns the correct port.
```
Q4. What is a MAC address table (CAM table)?
```
A MAC address table maps MAC addresses to physical ports on the switch. Example: Port 1 → AA:AA:AA:AA:AA:AA. The switch builds it automatically by watching incoming traffic — every time a device sends data, the switch records its MAC address and the port it came from.
```
Q5. What is a MAC Flooding attack?
```
An attacker floods the switch with thousands of fake MAC addresses, filling up the CAM table. Once full, the switch can no longer make forwarding decisions and starts broadcasting all incoming traffic to every port — turning it into a hub. The attacker can then sniff all traffic on the network. This is called a CAM Table Overflow Attack and the tool used is called Macof.
```


## Topic 4 — Router & NAT
Q1. What is a router and how is it different from a switch?
```
A router connects different networks together and routes traffic between them using IP addresses. A switch connects devices within the same network using MAC addresses.
```
Q2. Why does a router use IP addresses instead of MAC addresses?
```
MAC addresses only work within a local network — they are not designed to travel across the internet. IP addresses are globally routable, meaning every router in the world understands them. MAC is like your name (only recognized locally), IP is like your postal address (anyone anywhere can route to it).
```
Q3. Walk through what happens when you type google.com — router's role?
```
First DNS resolution converts google.com to an IP address. Then your browser sends a request to that IP. Your router receives it, replaces your private IP with your public IP using NAT, and forwards the request across the internet to Google's server. Google responds to your public IP, the router translates it back to your private IP and delivers the response to your device.
```
Q4. What is a routing table?
```
A routing table is a database stored in a router that contains: destination network, subnet mask, next hop (which router to forward to next), interface (which port to send out from), and metric (cost to determine best route). The router uses this to decide where to forward each packet.
```
Q5. How does your private IP reach Google if private IPs can't travel the internet?
```
The router uses NAT (Network Address Translation). It replaces your private IP (e.g. 192.168.1.5) with your public IP before sending data to the internet. When Google responds to the public IP, the router checks its NAT translation table and forwards the response back to the correct private device inside the LAN.
```


## Topic 5 — DNS
Q1. What is DNS and why does it exist?
```
DNS stands for Domain Name System. It translates human-readable domain names (like google.com) into IP addresses that machines understand. It exists because humans remember names easily but computers communicate using IP addresses.
```
Q2. Walk through the full DNS resolution process.

```
Browser checks its local cache — if found, uses cached IP directly. 2. If not cached, sends request to DNS Resolver (usually provided by ISP). 3. Resolver queries Root DNS Server — doesn't know the IP but knows which TLD server handles .com. 4. TLD Server for .com directs resolver to the Authoritative DNS Server for google.com. 5. Authoritative DNS Server returns the actual IP address. 6. Resolver caches the result and returns IP to browser. 7. Browser connects to Google's web server using that IP.
```

Q3. What are DNS records? Name 4 types.
```
DNS records are entries in DNS servers that define how a domain behaves. A Record: maps a domain to an IPv4 address. AAAA Record: maps a domain to an IPv6 address. CNAME Record: aliases one domain to another (e.g. www.google.com → google.com). MX Record: specifies mail servers for a domain. TXT Record: stores text information. NS Record: identifies the authoritative DNS server for a domain — heavily used in DNS enumeration attacks.
```
Q4. What is DNS caching and what is the security risk?
```
DNS caching temporarily stores DNS query results so the full resolution process doesn't need to repeat for every visit. Each record has a TTL (Time To Live) that determines how long it stays cached. The security risk is DNS Cache Poisoning — an attacker injects a fake malicious DNS record into a resolver's cache, redirecting all users of that resolver to a fake site until the TTL expires.
```
Q5. What is DNS Spoofing and how do you protect against it?
```
DNS Spoofing is when an attacker corrupts DNS records so a legitimate domain points to a malicious IP instead. The victim visits what looks like the real site but is actually a fake controlled by the attacker — leading to credential theft. Protection: always verify HTTPS, avoid clicking suspicious links, avoid public/untrusted WiFi, use DNSSEC-enabled domains, and use trusted DNS servers like Cloudflare (1.1.1.1) or Google (8.8.8.8).
```


## Topic 6 — HTTP & HTTPS
Q1. What is HTTP and what does it stand for?
```
HTTP stands for Hypertext Transfer Protocol. It is a request-response protocol used for communication between a web browser (client) and a web server. The client always initiates the request — the server only responds when asked.
```
Q2. Difference between HTTP and HTTPS?
```
HTTPS (HTTP Secure) adds SSL/TLS encryption to HTTP. This encrypts all data in transit so even if intercepted, it appears as unreadable gibberish. SSL is deprecated — TLS is the modern standard. The S in HTTPS stands for Secure.
```
Q3. Walk through an HTTP request and response cycle.
```
The browser sends an HTTP Request containing: method (GET/POST), path (/index.html), HTTP version, Host header, User-Agent (browser info), and Cookies. The server processes the request and sends an HTTP Response containing: status code (200 OK), Content-Type header, Set-Cookie header, and the actual content (HTML/JSON). Common HTTP methods — GET (retrieve), POST (send data), PUT (update), DELETE (remove), OPTIONS (ask what methods are allowed).
```
Q4. What are HTTP status codes? Give examples.
```
3-digit codes indicating the result of a request. 200 OK (success), 301 Moved Permanently (redirect), 400 Bad Request (malformed input), 401 Unauthorized (login required), 403 Forbidden (access denied — resource exists but blocked), 404 Not Found (resource doesn't exist), 500 Internal Server Error (server crashed — often triggered by SQLi attempts), 503 Service Unavailable (server overloaded — seen during DoS attacks).
```
Q5. What are the risks of HTTP? Give a real attack scenario.
```
HTTP transmits all data in plain text — no encryption. A MITM (Man-in-the-Middle) attacker on the same network can intercept all traffic. For example: a user logs into an HTTP site on public WiFi — the attacker captures the username and password in plain text using a tool like Wireshark.
```
Q6. You are on public WiFi on an HTTP site — what can an attacker see and do?
```
Because there is no encryption, an attacker on the same network can see: all websites you visit, all form data including usernames and passwords, cookies, and session IDs. Using the stolen session ID the attacker can perform Session Hijacking — impersonating you on a website without needing your password. This is done using tools like Wireshark or Ettercap.
```
