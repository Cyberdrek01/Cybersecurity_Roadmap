# Bandit Level 08 → Level 09

## 🎯 Objective
The password is stored in `data.txt` and is the **only line of text that occurs only once**.

---

## 💡 Concepts Covered

### Pipes in Linux
A **pipe** (`|`) takes the output of one command and feeds it as input to the next command. This lets you chain commands together:
```bash
command1 | command2 | command3
```

### The `sort` Command
Sorts lines in a file alphabetically or numerically:
```bash
sort filename
```
Identical lines will be grouped together after sorting — essential before using `uniq`.

### The `uniq` Command
Removes or reports duplicate lines. **Important:** `uniq` only detects duplicates on adjacent lines — so always `sort` first.

| Flag | Meaning |
|---|---|
| `uniq` | Remove duplicate adjacent lines |
| `uniq -c` | Count occurrences of each line |
| `uniq -d` | Show only duplicate lines |
| `uniq -u` | Show only unique lines (appears exactly once) |

### Combining sort and uniq
```bash
sort data.txt | uniq -u
```
This:
1. Sorts all lines — duplicates become adjacent
2. `uniq -u` shows only lines that appear exactly once

---

## 🔧 Solution

### Command Used
```bash
sort data.txt | uniq -u
```

### Steps
1. Logged in as `bandit8`
2. Ran `cat data.txt` — thousands of lines, mostly duplicates
3. Used `sort` to group identical lines together
4. Piped to `uniq -u` to show only the line that appears once
5. One line returned — that was the password

### What the Output Looks Like
```
bandit8@bandit:~$ sort data.txt | uniq -u
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```

---

## 🧠 What I Learned

- Pipes (`|`) chain commands together — output of one becomes input of the next
- `sort` must come before `uniq` because `uniq` only detects adjacent duplicates
- `uniq -u` shows only lines that appear exactly once
- `uniq -c` counts occurrences — useful for finding the most common entries
- Piping is one of the most powerful Linux concepts

---

## 🔐 Cybersecurity Relevance

**Log analysis with sort and uniq:**
```bash
# Find most common IPs in a log file
cat access.log | grep -o '[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+' | sort | uniq -c | sort -rn | head

# Find unique failed login usernames
grep "Failed password" auth.log | awk '{print $9}' | sort | uniq -c | sort -rn

# Find unique destination ports in firewall logs
cat firewall.log | awk '{print $dest_port}' | sort | uniq -c | sort -rn
```

This exact pattern — `sort | uniq -c | sort -rn` — is used constantly in SOC work to find the most frequent or unusual entries in logs. It is a fundamental log analysis technique.

---

*Part of [Bandit Writeups](./README.md)*
