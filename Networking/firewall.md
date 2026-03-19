# Firewall
A firewall is a network security system, available as hardware and software, that moditors the incoming and outgoing traffic in a system based on predefined rules. It acts like a security guard, filtering data packets to either:

* Accept: Allow the traffic.
* Reject: Block with an error response.
* Drop: Block silently without response.

## Importance of Firewall: 

* Prevent Unauthorized Access.
* Block Malicious Traffic.
* Protect Sensitive Information.
* Control Network Usage.
* Mitigate Insider Risks.

## Working of Firewall: 

1. All data packets entering or leaving a system must pass through the firewall.
2. The firewall examines each packet against predefined security rules set by the organization.
3. If the packets matches the safe rules than it is allowed to pass, or else if it is suspicious, blacklisted or contains malicious content then it is blocked.
4.  Blocked or unusual traffic is examined thhrough logs, and real time alert may be generated for serious threats.
5.  Since it is not possible to define every rule, the firewall applies a default policy.

## Types of firewall:

1. Packet Filtering Firewall (Stateless)

Works at Network Layer

Checks:

IP address

Port number

Protocol

Does NOT remember past traffic

👉 Fast but less secure

🔹 2. Stateful Inspection Firewall

Keeps track of active connections

Allows only valid responses

👉 Example:

If you request a website → response allowed

Random incoming packet → blocked

✔ More secure than stateless

🔹 3. Proxy Firewall (Application Layer)

Acts as intermediary

Client never talks directly to server

👉 Features:

Content filtering

URL blocking

✔ High security but slower

🔹 4. Next-Generation Firewall (NGFW) (bonus)

Deep Packet Inspection (DPI)

Intrusion Prevention (IPS)

Application-level filtering

🔹 5. Web Application Firewall (WAF) (bonus)

Protects web apps from:

SQL Injection

XSS Software Firewall
