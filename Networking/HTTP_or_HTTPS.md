# HTTP/HTTPS
Hyper Text Transfer Protocol and S for security (SSL encryption) is a common protocol for sending data between a web browser and a website. We should use HTTPS because it ensures that the data is encrypted for added security.

## Working of HTTPS

Step1: Client Hello.
* Browser says: “Let’s start secure communication”
Step 2: Server sends Certificate.
* Contains:
  * Public key.
  * Doman Name.
  * Signed by trusted authority.
Step 3: Verification.
* Browser checks validity.
Step 4: Key Exchange.
* Secure session key is created.
Step 5: Encrypted Communication.
* All data now encrypted.
 
## Why HTTPS Matters and What Happens Without It?
HTTPS is important because it keeps the information on websites safe from being easily viewed or stolen by anyone who might be spying on the network.
When a website uses regular HTTP, data is sent in small chunks called packets that can easily be intercepted using free software.
This makes communication, especially over public Wi-Fi, very vulnerable to attacks.
On the other hand, HTTPS encrypts the data, so even if someone manages to intercept the packets, they will appear as random, unreadable characters.

Example: 
```bash
Before encryption: "This is a string of text that is completely readable"
After encryption: "ITM0IRyiEhVpa6VnKyExMiEgNveroyWBPlgGyfkflYjDaaFf/Kn3bo3OfghBPDWo6AfSHlNtL8N7ITEwIXc1gU5X73xMsJormzzXlwOyrCs+9XCPk63Y+z0="
```
## Secure Socket Layer (SSL)
The main responsibility of SSL is to ensure that the data transfer between the communicating systems is secure and reliable. It is the standard security technology that is used for encryption and decryption of data during the transmission of requests.

HTTPS is basically the same old HTTP but with SSL.
For establishing a secure communication link between the communicating devices, SSL uses a digital certificate called SSL certificate. 
### Roles of the SSL layer
* Ensuring that the browser communicates with the required server directly.
* Ensuring that only the communicating systems have access to the messages they exchange.

## Encryption in HTTPS
HTTP transfers data in a hypertext format between the browser and the web server, whereas HTTPS transfers data in an encrypted format. As a result, HTTPS protects websites from having their information broadcast in a way that anyone eavesdropping on the network can easily see.

During the transit between the browser and the web server, HTTPS protects the data from being accessed and altered by hackers.
Even if the transmission is intercepted, hackers will be unable to use it because the message is encrypted.
It uses an asymmetric public key infrastructure for securing a communication link.

| HTTP | HTTPS |
|---|---|
| HTTP stands for HyperText Transfer Protocol|HTTPS stands for HyperText Transfer Protocol Secure. |
| HTTP Works at the Application Layer. | Also workds at application layer but with SSL and TLS encryption. |
| Data is sent in plain text. | Data is encrypted using TLS. |
| Encryption is provided. | No encryption, no security. |
