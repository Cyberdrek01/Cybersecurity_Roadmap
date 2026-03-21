# Bandit Level 05 → Level 06

## 🎯 Objective
The password is stored somewhere under the `inhere` directory with all of the following properties:
- Human-readable
- 1033 bytes in size
- Not executable

---

## 💡 Concepts Covered

### The `find` Command
`find` is one of the most powerful Linux commands. It searches for files based on specific properties — name, size, type, permissions, owner, and more.

### Basic `find` Syntax
```bash
find [where to search] [conditions]
```

### Useful `find` Flags

| Flag | Meaning | Example |
|---|---|---|
| `-type f` | Regular file | `find . -type f` |
| `-type d` | Directory | `find . -type d` |
| `-size` | File size | `find . -size 1033c` |
| `-readable` | Human-readable file | `find . -readable` |
| `! -executable` | NOT executable | `find . ! -executable` |
| `-name` | File name | `find . -name "*.txt"` |

### Size Units in `find`
| Unit | Meaning |
|---|---|
| `c` | Bytes |
| `k` | Kilobytes |
| `M` | Megabytes |
| `G` | Gigabytes |

So `1033c` means exactly 1033 bytes.

### Combining Multiple Conditions
`find` can combine multiple conditions — all must be true for a file to match:
```bash
find . -type f -size 1033c -readable ! -executable
```
This finds files that are: a regular file AND 1033 bytes AND readable AND NOT executable.

---

## 🔧 Solution

### Command Used
```bash
find . -type f -size 1033c -readable ! -executable
```

### Steps
1. Logged in as `bandit5`
2. Ran `cd inhere` — saw many subdirectories
3. Manually checking each would take too long
4. Used `find` with all three conditions at once
5. One file matched — used `cat` to read the password

### What the Output Looks Like
```
bandit5@bandit:~$ find . -type f -size 1033c -readable ! -executable
./inhere/maybehere07/.file2
bandit5@bandit:~$ cat ./inhere/maybehere07/.file2
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
```

---

## 🧠 What I Learned

- `find` searches for files based on specific properties
- `-size 1033c` finds files of exactly 1033 bytes (`c` = bytes)
- `! -executable` excludes executable files (the `!` means NOT)
- `-readable` filters for human-readable files
- Combining multiple `find` conditions narrows results to exactly what you need
- `find .` searches from the current directory recursively through all subdirectories

---

## 🔐 Cybersecurity Relevance

`find` is one of the most used commands in both offensive and defensive security:

**In pentesting / post-exploitation:**
```bash
find / -name "*.conf" 2>/dev/null          # find all config files
find / -perm -4000 2>/dev/null             # find SUID files (privilege escalation)
find / -writable -type f 2>/dev/null       # find writable files
find / -name "id_rsa" 2>/dev/null          # find SSH private keys
find / -name "*.log" 2>/dev/null           # find log files
```

**In incident response:**
```bash
find / -newer /var/log/auth.log            # files modified after a specific time
find /tmp -type f -executable              # suspicious executables in /tmp
find / -name ".bash_history"               # find command history files
```

`find` with SUID permissions (`-perm -4000`) is especially important — SUID files run with elevated privileges and are a common privilege escalation vector.

---

*Part of [Bandit Writeups](./README.md)*
