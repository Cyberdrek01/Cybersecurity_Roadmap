# Bandit Level 09 → Level 10

## 🎯 Objective
The password is stored in `data.txt` in one of the few human-readable strings, preceded by several `=` characters.

---

## 💡 Concepts Covered

### The `strings` Command
`strings` extracts all human-readable text from a file — including binary files. It looks for sequences of printable characters (minimum 4 characters by default).

```bash
strings filename
```

`data.txt` is mostly binary data with a few readable strings hidden inside. `cat` would show mostly gibberish — `strings` extracts only the readable parts.

### Combining `strings` with `grep`
Since the password is preceded by `=` characters, pipe `strings` output into `grep`:
```bash
strings data.txt | grep "==="
```

---

## 🔧 Solution

### Command Used
```bash
strings data.txt | grep "=="
```

### Steps
1. Logged in as `bandit9`
2. Ran `cat data.txt` — mostly binary gibberish
3. Used `strings` to extract readable text
4. Piped to `grep "=="` to find lines with multiple `=` characters
5. Password was on the line preceded by `========`

### What the Output Looks Like
```
bandit9@bandit:~$ strings data.txt | grep "=="
========== the
========== password
========== is
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```

---

## 🧠 What I Learned

- `strings` extracts readable text from any file including binary files
- Binary files contain mostly non-printable characters — `strings` filters these out
- Combining `strings` with `grep` narrows down results instantly
- This technique works on executables, images, and any binary file

---

## 🔐 Cybersecurity Relevance

`strings` is a fundamental **malware analysis** tool:

**Analysing suspicious files:**
```bash
strings malware.exe | grep -i "http"           # find URLs in malware
strings malware.exe | grep -i "password"       # find hardcoded passwords
strings malware.exe | grep -i "cmd"            # find command execution strings
strings malware.exe | grep "[0-9]\{1,3\}\.[0-9]\{1,3\}"  # find IP addresses
```

**What analysts look for in malware strings:**
- URLs and IP addresses — C2 server communication
- File paths — where malware writes files
- Registry keys — persistence mechanisms
- Hardcoded credentials — backdoor access
- Error messages — reveal functionality

`strings` is often the very first command run on an unknown suspicious file — it gives a quick overview of what the file might do before any deeper analysis.

---

*Part of [Bandit Writeups](./README.md)*
