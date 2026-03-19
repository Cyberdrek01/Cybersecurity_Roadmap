# TCP and UDP

TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) are two core protocols of the Transport Layer of the OSI and TCP/IP models. Both are responsible for end-to-end communication between applications, but they differ significantly in terms of reliability, speed, and use cases.

# Transmission Control Protocol (TCP)
TCP is a reliable, connection-oriented transport protocol that ensures accurate and ordered data delivery. It uses control mechanisms to guarantee data correctness, which makes it slower but dependable.

* Connection-oriented protocol
* Reliable and ordered data delivery
* Higher overhead but high accuracy

# User Datagram Protocol (UDP)
UDP is a fast, connectionless transport protocol that sends data without reliability guarantees. It is efficient for applications where speed is more important than accuracy.

* Connectionless and lightweight
* No guarantee of delivery or order
* Low overhead and high speed

# Difference between TCP and UDP 

| Transmission Control Protocol | User Datagram Protocol |
| -------- | -------- |
| Connection-oriented; uses a three-way handshake | Connectionless; no handshake |
| Guarantees reliable data delivery	| Does not guarantee delivery |
| Uses acknowledgements (ACKs) | No acknowledgements |
| Supports retransmission of lost packets |	No retransmission support |
| Used by HTTP, HTTPS, FTP, SMTP	| Used by DNS, DHCP, VoIP, Streaming |
