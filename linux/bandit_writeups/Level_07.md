# Bandit Level 06 → Level 07

## 🎯 Objective
The password is stored somewhere on the server with all of the following properties:
- Owned by user **bandit7**
- Owned by group **bandit6**
- **33 bytes** in size

---

## 💡 Concepts Covered

### Finding Files by Owner and Group
The `find` command can search by file ownership:

| Flag | Meaning | Example |
|---|---|---|
| `-user` | File owned by specific user | `find / -user bandit7` |
| `-group` | File owned by specific group | `find / -group bandit6` |
| `-size 33c` | File is exactly 33 bytes | `find / -size 33c` |

### Searching the Entire Server
This level requires searching the **entire server** not just the home directory. Use `/` as the starting point:
```bash
find / -user bandit7 -group bandit6 -size 33c
```

### Suppressing Permission Errors
Searching `/` generates many "Permission denied" errors for directories you cannot access. Suppress them by redirecting errors to `/dev/null`:
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

`2>/dev/null` means — send all error messages (file descriptor 2) to `/dev/null` (the Linux black hole — discards everything sent to it).

---

## 🔧 Solution

### Command Used
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

### Steps
1. Logged in as `bandit6`
2. Home directory was empty — file is somewhere on the entire server
3. Used `find /` to search from root
4. Added `-user bandit7 -group bandit6 -size 33c` to match all conditions
5. Added `2>/dev/null` to hide permission errors
6. One file matched — used `cat` to read the password

### What the Output Looks Like
```
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S
```

---

## 🧠 What I Learned

- `find /` searches the entire filesystem from root
- `-user` and `-group` flags filter by file ownership
- `2>/dev/null` suppresses error messages — essential when searching system directories
- `/dev/null` is Linux's discard bin — anything sent there disappears
- File descriptor 2 = stderr (error output), 1 = stdout (normal output)

---

## 🔐 Cybersecurity Relevance

**In pentesting — post exploitation:**
After gaining access to a system, attackers search for sensitive files owned by privileged users:
```bash
find / -user root -readable 2>/dev/null        # files owned by root that you can read
find / -group shadow -readable 2>/dev/null     # shadow password file access
find / -user www-data 2>/dev/null              # files owned by web server user
```

**In SOC / incident response:**
- Unusual file ownership can indicate tampering
- Files owned by privileged users in unexpected locations are suspicious
- `2>/dev/null` is used constantly when running recon commands on systems

---

*Part of [Bandit Writeups](./README.md)*
