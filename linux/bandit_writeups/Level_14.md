# Bandit Level 13 → Level 14

## 🎯 Objective
Instead of a password, this level provides a **private SSH key** that can be used to log into bandit14. The password is stored in `/etc/bandit_pass/bandit14` and can only be read by user bandit14.

---

## 💡 Concepts Covered

### SSH Key Authentication
SSH supports two authentication methods:
1. **Password authentication** — enter a password when connecting
2. **Key-based authentication** — use a cryptographic key pair (more secure)

### How SSH Key Pairs Work
```
Private Key (kept secret)     Public Key (shared with server)
      │                               │
      └───────────── pair ────────────┘

Server stores: Public Key
You hold:      Private Key

When connecting:
→ Server sends a challenge encrypted with your Public Key
→ Only your Private Key can decrypt it
→ If successful → you are authenticated
→ No password needed
```

**Why key authentication is more secure:**
- No password to brute force
- Private key never leaves your machine
- Even if the server is compromised, your private key is safe

### Using a Private Key with SSH
```bash
ssh -i private_key_file username@hostname -p port
```
The `-i` flag specifies the identity file (private key).

### Key File Permissions
SSH requires private key files to have strict permissions — only the owner can read it:
```bash
chmod 600 sshkey.private    # owner read/write only
chmod 400 sshkey.private    # owner read only (also acceptable)
```
If permissions are too open, SSH refuses to use the key with a warning.

---

## 🔧 Solution

### Commands Used
```bash
ls
ssh -i sshkey.private bandit14@localhost -p 2220
cat /etc/bandit_pass/bandit14
```

### Steps
1. Logged in as `bandit13`
2. Ran `ls` — found `sshkey.private` in home directory
3. Used SSH with `-i` flag to connect as bandit14 using the key
4. Connected to `localhost` (same machine) as bandit14
5. Read the password from `/etc/bandit_pass/bandit14`

### What the Output Looks Like
```
bandit13@bandit:~$ ls
sshkey.private
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p 2220
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
```

---

## 🧠 What I Learned

- SSH key pairs — public key on server, private key held locally
- `-i` flag specifies which private key to use for SSH
- `localhost` means the same machine you are currently on (127.0.0.1)
- `/etc/bandit_pass/` stores passwords — only readable by the corresponding user
- Key-based authentication is more secure than password authentication

---

## 🔐 Cybersecurity Relevance

SSH keys are critical in both attack and defence:

**For attackers:**
- Finding an unprotected private key on a compromised system = instant access to other servers
- Common locations to check for SSH keys:
```bash
cat ~/.ssh/id_rsa              # default private key location
cat ~/.ssh/authorized_keys     # shows who can connect
find / -name "id_rsa" 2>/dev/null    # find any private keys
find / -name "*.pem" 2>/dev/null     # find PEM key files
```

**For defenders/SOC:**
- Monitor `~/.ssh/authorized_keys` — attackers add their own public key as a backdoor
- Unauthorised keys in `authorized_keys` = persistent backdoor even after password change
- Audit SSH key usage — who has keys to what servers

**For blue team — hardening SSH:**
```bash
# Disable password authentication — force key only
PasswordAuthentication no

# Disable root login
PermitRootLogin no
```

---

*Part of [Bandit Writeups](./README.md)*
