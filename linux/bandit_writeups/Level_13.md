# Bandit Level 12 → Level 13

## 🎯 Objective
The password is stored in `data.txt` which is a **hexdump of a file that has been repeatedly compressed**. The file must be decompressed multiple times to retrieve the password.

---

## 💡 Concepts Covered

### What is a Hex Dump?
A hex dump is a representation of binary data in hexadecimal format — used to view binary files as readable text.

```
00000000: 1f8b 0808 dfcd eb65 0203 6461 7461 322e  .......e..data2.
00000010: 6269 6e00 013e 02c1 fd42 5a68 3931 4159  bin..>...BZh91AY
```

The `xxd` command creates hex dumps. To reverse a hex dump back to binary:
```bash
xxd -r hexdump_file > binary_file
```

### Compression Tools

| Tool | File Extension | Compress | Decompress |
|---|---|---|---|
| gzip | `.gz` | `gzip file` | `gzip -d file.gz` or `gunzip file.gz` |
| bzip2 | `.bz2` | `bzip2 file` | `bzip2 -d file.bz2` or `bunzip2 file.bz2` |
| tar | `.tar` | `tar -cf archive.tar files` | `tar -xf archive.tar` |

### Identifying File Types
Use `file` command after each decompression to identify what type the result is:
```bash
file data
```

### Working in /tmp
Since the home directory is read-only, work is done in `/tmp`:
```bash
mktemp -d          # creates a unique temporary directory
cp data.txt /tmp/  # copy file to tmp
cd /tmp/tmpXXXXXX  # work here
```

---

## 🔧 Solution

### Commands Used
```bash
mktemp -d
cp data.txt /tmp/tmp.XXXXXX/
cd /tmp/tmp.XXXXXX/
xxd -r data.txt > data
file data
# Repeated decompression based on file type output:
gzip -d data.gz
bzip2 -d data.bz2
tar -xf data.tar
# ... repeated multiple times until ASCII text found
cat final_file
```

### Steps
1. Logged in as `bandit12`
2. Created a temporary working directory with `mktemp -d`
3. Copied `data.txt` to the temp directory
4. Converted hex dump back to binary using `xxd -r`
5. Used `file` to check what type the binary was
6. Decompressed using the appropriate tool
7. Checked type again with `file`
8. Repeated decompression many times (gzip → bzip2 → tar → gzip → bzip2...)
9. Eventually got an ASCII text file containing the password

### What the Process Looks Like
```
bandit12@bandit:~$ mktemp -d
/tmp/tmp.KsHd3G
bandit12@bandit:~$ cp data.txt /tmp/tmp.KsHd3G/
bandit12@bandit:~$ cd /tmp/tmp.KsHd3G/
bandit12@bandit:/tmp/tmp.KsHd3G$ xxd -r data.txt > data
bandit12@bandit:/tmp/tmp.KsHd3G$ file data
data: gzip compressed data
bandit12@bandit:/tmp/tmp.KsHd3G$ mv data data.gz && gzip -d data.gz
bandit12@bandit:/tmp/tmp.KsHd3G$ file data
data: bzip2 compressed data
bandit12@bandit:/tmp/tmp.KsHd3G$ mv data data.bz2 && bzip2 -d data.bz2
... (repeated many times)
bandit12@bandit:/tmp/tmp.KsHd3G$ file data
data: ASCII text
bandit12@bandit:/tmp/tmp.KsHd3G$ cat data
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```

---

## 🧠 What I Learned

- `xxd -r` converts a hex dump back to binary data
- `file` command identifies compression type — must be used after each step
- Files must be renamed with correct extension before decompression tools work
- `mktemp -d` creates a safe temporary working directory
- Multiple layers of compression is a common technique to obscure data
- Each compression format has its own tool — gzip, bzip2, tar

---

## 💭 What Was Tricky
This was the hardest level so far. The file was compressed many times in different formats. Had to use multiple sources to understand the full process. The key insight was to always run `file` after each decompression to know which tool to use next.

---

## 🔐 Cybersecurity Relevance

**Malware analysis:**
- Malware is frequently compressed and packed multiple times to evade antivirus detection
- Analysts must decompress and unpack malware layer by layer to reach the actual executable
- This process is called **unpacking** or **deobfuscation**

**Forensics:**
- Evidence files may be compressed — understanding compression tools is essential
- Hex dumps are used in forensic tools to inspect raw disk data
- `xxd` is used to examine file headers and identify true file types

**Key concept — magic bytes:**
Every file type has a signature in its first few bytes called **magic bytes**:
```
gzip:  1f 8b
bzip2: 42 5a 68 (BZh)
PNG:   89 50 4e 47
PDF:   25 50 44 46 (%PDF)
```
Security tools use magic bytes to identify files regardless of their extension — the same way `file` command works.

---

*Part of [Bandit Writeups](./README.md)*
