# OSI & TCP/IP Model

## OSI- Open System Interconnection 

### The 7 Layers (Top → Bottom)
```bash
  7️⃣ Application
  6️⃣ Presentation
  5️⃣ Session
  4️⃣ Transport
  3️⃣ Network
  2️⃣ Data Link
  1️⃣ Physical
```

#### 1. Application Layer

This is the top most layer in the OSI model.
It is what the user sees.
Here various protocols work like-

* HTTP/HTTPS- Hyper Text Transfer Protocol S for secure http lacks security as all data transmits in plain text ao https should be used for better security. HTTP+SSL/TLS.
* FTP- File Transfer Protocol, it is insecure, unencrypted, legacy protocol data and password in plain text, making it vulnerable to brute force or Main in The Middle attacks. Organization should use SFTP for more security.
* DNS- Domain Name System, Convert the domain names into IP addresses.
* SMTP- Simple Mail Transfer Protocol, used to send mails but it lacks security so SMTP+TLS for encryption.
* SSH- Secure Shell, enables secure,encrypted communication over a network between two devices.
  
This is where web attacks happen-

* SQL injection-
* XSS-
* File upload attacks-

