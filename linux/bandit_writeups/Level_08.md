# Bandit Level 07 → Level 08

## 🎯 Objective
The password is stored in `data.txt` next to the word **millionth**.

---

## 💡 Concepts Covered

### The `grep` Command
`grep` searches for a specific pattern or word inside a file and returns the matching lines.

```bash
grep "word" filename
```

`data.txt` contains millions of lines — manually reading it is impossible. `grep` instantly finds the line containing "millionth".

### Basic `grep` Syntax
```bash
grep "pattern" filename
```

### Useful `grep` Flags
| Flag | Meaning |
|---|---|
| `-i` | Case insensitive search |
| `-n` | Show line numbers |
| `-r` | Search recursively in directories |
| `-v` | Invert — show lines that do NOT match |
| `-c` | Count matching lines |

---

## 🔧 Solution

### Command Used
```bash
grep "millionth" data.txt
```

### Steps
1. Logged in as `bandit7`
2. Ran `ls` — saw `data.txt`
3. Tried `cat data.txt` — file was enormous, scrolled forever
4. Used `grep "millionth" data.txt` to find the exact line
5. Output showed the word millionth followed by the password

### What the Output Looks Like
```
bandit7@bandit:~$ grep "millionth" data.txt
millionth       TESKZC20W71sNiMJmwDeBiyx6rDYjavGn
```

---

## 🧠 What I Learned

- `grep` searches for patterns inside files instantly — no matter how large the file
- Much faster than reading large files manually
- `grep` returns the entire line containing the match
- This is the same technique used to search log files in SOC work

---

## 🔐 Cybersecurity Relevance

`grep` is one of the most used commands in SOC and security work:

**Log analysis:**
```bash
grep "Failed password" /var/log/auth.log          # find failed SSH logins
grep "ACCEPT" /var/log/firewall.log               # find allowed connections
grep "192.168.1.100" /var/log/apache2/access.log  # find all requests from an IP
grep -i "error" /var/log/syslog                   # find all errors
```

**Incident response:**
```bash
grep -r "malicious-domain.com" /var/log/          # search all logs for a domain
grep -r "cmd.exe" /var/log/apache2/               # find command injection attempts
grep "sudo" ~/.bash_history                       # find sudo commands in history
```

`grep` through log files is one of the most fundamental SOC analyst skills — you will use it every single day.

---

*Part of [Bandit Writeups](./README.md)*
