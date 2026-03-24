# Bandit Level 16 → Level 17

## 🧩 Level Goal

Retrieve the credentials for the next level by submitting the current level password to a service running on a port between **31000–32000** on localhost.

---

## 🛠️ Tools Used

* `nmap`
* `openssl s_client`
* `ssh`

---

## 🔍 Step 1: Scan for Open Ports

```bash
nmap -p 31000-32000 localhost
```

### Example Output:

```
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31790/tcp open  unknown
```

---

## 🔐 Step 2: Identify SSL-enabled Port

Test each open port using:

```bash
openssl s_client -connect localhost:<port>
```

Example:

```bash
openssl s_client -connect localhost:31790
```

👉 Only one port will successfully establish an SSL/TLS connection.

---

## 📤 Step 3: Submit Password

Once connected to the correct port:

1. Paste the current level password
2. Press Enter

If correct, the server returns a **private RSA key**

---

## 💾 Step 4: Save the Private Key

```bash
nano key17
```

Paste the key:

```
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

---

## 🔑 Step 5: Set Permissions

```bash
chmod 600 key17
```

---

## 🚀 Step 6: Login to Next Level

```bash
ssh -i key17 bandit17@localhost -p 2220
```

---

## ⚠️ Notes

* Messages like `DONE`, `RENEGOTIATING`, or `KEYUPDATE` may appear.
* These are normal SSL behaviors — just press Enter or resend the password.

---

## ✅ Result

Successfully logged into **Bandit Level 17** using the retrieved private key.

---

## 🎯 Key Learning

* Port scanning using `nmap`
* Identifying SSL services
* Using `openssl s_client`
* Handling private keys for SSH authentication
