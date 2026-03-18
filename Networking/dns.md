# DNS - Domain Name System

In simple term DNS is a system that translates/converts the human readable domain names into IP addresses.
```bash
https://google.com->8.8.8.8
```
* Uses root, TLD, and authoritative DNS servers for resolution.
* Supports caching to improve speed and reduce network traffic.

## Working of DNS:

1. User enter a website address into the web browser
2. Checks local cache - The browser checks the local cache whether the domain was recently looked up or not. If it finds the corresponding IP address, it uses that directly without querying external servers.
3. DNS resolver query: If the IP isn't in the local cache, your computer sends request to a DNS resolver. (DNS helps find particular ip related to particular domain name).
4. Root DNS Server: Resolver sends request to a root dns server. The root server doesnot know the exact ip for www.google.com but knows which Top Level Domain (TLD) server to query based on domain extension (eg: .com,.org).
5. TLD server: The TLD server for .com directs the resolver to the authoritative DNS server for google.com.
6. Authoritative DNS Server: This server holds the actual DNS records for google.com, including the IP address of the website’s server. It sends this IP address back to the resolver.
7. Final Response: The DNS resolver sends the IP address to your computer, allowing it to connect to the website’s server and load the page.

## Structure of DNS
DNS operates through a hierarchical structure, ensuring scalability and reliability across the global internet infrastructure. Here's how it’s organized:

Root DNS Servers: These are the highest-level DNS servers and know where to find the TLD servers. They are crucial for directing DNS queries to the correct locations.
TLD Servers: These servers manage domain extensions like .com, .org, .net, .edu, .gov and others. They help route queries to the authoritative DNS servers for specific domains.
Authoritative DNS Servers: These are the servers that store the actual DNS records for domain names. They are responsible for providing the correct IP addresses that allow users to reach websites.

## Domain Name Server
The client machine sends a request to the local name server, which, if the root does not find the address in its database, sends a request to the root name server, which in turn, will route the query to a top-level domain (TLD) or authoritative name server.

* The root name server can also contain some hostName to IP address mappings.
* The Top-level domain (TLD) server always knows who the authoritative name server is.
* So finally the IP address is returned to the local name server which in turn returns the IP address to the host.

## DNS Lookup
DNS Lookup, also called DNS Resolution, is the process of translating a human-readable domain name into its corresponding IP address. The process involves:

* DNS Resolver: Initiates the lookup process. Also called a DNS client.
* Recursive Query: Resolver queries multiple servers on behalf of the client until the IP is found.
* Iterative Query: Resolver asks servers for the best answer available.
* Non-Recursive Query: Resolver queries a server that already has the record in its cache.

