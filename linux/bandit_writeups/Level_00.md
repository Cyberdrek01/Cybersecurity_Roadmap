# Bandit Level 00 → Level 01

## 🎯 Objective
Log into the Bandit game server for the first time using SSH with the provided credentials.

---

## 📋 Level Details

| Field | Details |
|---|---|
| Host | bandit.labs.overthewire.org |
| Port | 2220 |
| Username | bandit0 |
| Password | bandit0 |

---

## 💡 Concepts Covered

### What is SSH?
**SSH (Secure Shell)** is a cryptographic network protocol used to securely connect to a remote system over an unsecured network. It encrypts all communication between your machine and the remote server.

SSH is one of the most important tools in cybersecurity:
- System administrators use it to manage remote servers
- Pentesters use it to maintain access after exploitation
- SOC analysts use it to investigate remote systems
- Developers use it to deploy code to servers

### SSH Command Syntax
```bash
ssh username@hostname -p port
```

| Part | Meaning |
|---|---|
| `ssh` | Secure Shell command |
| `username` | The account to log into |
| `@hostname` | The remote server address |
| `-p port` | The port number (default SSH port is 22) |

**Why port 2220?**
The default SSH port is **22**. OverTheWire uses port **2220** as a non-standard port — this is common in real environments where admins move SSH off port 22 to reduce automated brute force attacks.

---

## 🔧 Solution

### Command Used
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

### Steps
1. Opened the terminal
2. Typed the SSH command with the correct host, username, and port
3. When prompted — typed the password: `bandit0`
4. Successfully logged into the Bandit server

### What Success Looks Like
```
This is a OverTheWire game server.
More information on http://www.overthewire.org/wargames

bandit0@bandit:~$
```

The `bandit0@bandit:~$` prompt confirms you are now logged in as user `bandit0` on the remote server.

---

## 🧠 What I Learned

- How to use the `ssh` command to connect to a remote server
- SSH syntax — username, hostname, and port flag `-p`
- Why non-standard ports are used for SSH in real environments
- The difference between default port 22 and custom ports

---

## 🔐 Cybersecurity Relevance

**SSH in pentesting:**
Once an attacker gains valid credentials, SSH is often used to establish persistent remote access to a compromised server. This is why:
- Weak SSH passwords are a critical vulnerability
- SSH brute force attacks target port 22 (and other common ports)
- SOC analysts monitor for unusual SSH login attempts in logs

**What to look for in SOC:**
- Failed SSH login attempts (brute force indicator)
- SSH logins from unusual geographic locations
- SSH connections at unusual times (after hours)
- Multiple SSH sessions from the same IP to different servers (lateral movement)

---

*Part of [Bandit Writeups](./README.md)*
