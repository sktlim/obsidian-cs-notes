> Directory service that translates **hostnames** to **IP addresses**

## Key Services
- DNS is a distributed database implemented in a hierarchy of DNS servers 
- DNS is also a protocol that allows hosts to query the distributed database
- **Host Aliasing:**
	- Host can have 1 or more alias names e.g. www.google.com and google.com
	- Host will have a **canonical hostname** (see [[Fundamentals/Networks/Terminology|Terminology]]) e.g. abc.google.com
	- DNS can be invoked to find canonical hostname and IP address using alias hostnames 
- **Mail Server Aliasing:**
	- Canonical hostname of a mail server might be more complicated than simply `gmail.com`
	- DNS can be used by mail app to find canonical hostname and IP address using alias hostnames
	- MX record permits company's mail server and web server to have identical aliased hostnames
- **Load Distribution**
	- Busy sites are typically replicated over multiple servers, with each server running on different end system and IP address
	- A set of IP addresses are therefore associated with 1 canonical hostname
	- When clients make a query to DNS for the set of these IP addresses, server responds but rotates the order of the addresses with each reply, which distributes traffic among the addresses 
## Overview
- DNS servers are often UNIX machines running **Berkeley Internet Name Domain (BIND)** software
- Commonly employed by other app-layer protocols to translate hostname to IP address

## Properties
- DNS runs over UDP and uses port 53 

## Process: Hostname-to-IP translation
1) App on user's host needs to translate hostname `www.google.com` to IP address
2) App invokes client side DNS, specifying hostname to be translated
	1) `gethostbyname()` on UNIX does this
3) Client side DNS sends query to DNS database through UDP datagrams on port 53
4) DNS in user's host receives reply that provides desired mapping, which is passed to invoking application

### DNS Hierarchy
- To deal with scaling, DNS distributes its mapping across multiple servers around the world
- 3 classes of DNS servers
	- root DNS
	- top-level-domain (TLD) DNS servers e.g. com, org, edu
	- Authoritative DNS servers 
![[DNS Hierarchy.png]]
- Client contacts root DNS server, which returns IP address for TLD servers for TLD `com` 
- Client then contacts TLD server, which returns IP address for Authoritative server for `www.google.com`

- **Note:** There is another type of DNS server known as the **local DNS** server
	- Each ISP has a local DNS server (aka default name server), and when a host connects to an ISP, the ISP provides host with IP address of 1 or more of its local DNS server through [[Dynamic Host Configuration Protocol (DHCP)]] 
	- Local DNS server acts a proxy to forward DNS requests from host 

![[Example DNS query.png|350]]![[Example DNS Query 2.png|344]]
- DNS queries can be iterative (e.g. Fig 2.19 local DNS queries 2-7) or recursive (Fig 2.19 host queries 1 and 8, Fig 2.20 queries 1,2,7,8)

## DNS Caching
- **Goal:** Improve delay performance and reduce total number of queries
- When DNS server receives DNS reply, it caches mapping in local memory
- DNS server discards info after time to live (TTL) expires (typically 2 days according to textbook)
- Because of caching, root servers are bypassed

### Cache Invalidation
- **Automatic Expiry:** When TTL expires
- **Manual clearing:** Manually flush DNS records
	- Windows: `ipconfig /flushdns`
	- Linux: `sudo systemd-resolve --flush-caches`

## DNS Records and Messages
- DNS servers store **resource records (RR)**
- Each DNS reply message carries 1 or more RRs

### Resource Records (RR)
> Tuple containing (Name, Value, Type, TTL)
- Types
	- `A`: Standard domain-to-IP mapping
		- Name = hostname, 
		- Value = IP address for hostname 
	- `NS`: Route DNS queries further along the chain
		- Name = domain e.g. (foo.com)
		- Value = Hostname of authoritative DNS that knows how to obtain IP address for hosts in domain
	- `CNAME`: Provide querying hosts the canonical name for a hostname
		- Name = alias hostname
		- Value = canonical hostname
	- `MX`: Allows hostname of **mail servers** to have simple names
		- Company can have same aliased name for mail server and other servers since DNS will query MX record when looking for mail server and CNAME when looking for other server
		- Name = alias hostname
		- Value = Canonical hostname
- Time to Live (TTL): How long record should be cached for

### DNS Messages
![[DNS Message Format.png]]
- **Header section:** first 12 bytes
	- **Identification:** 16-bit number identifying query; response will have same identification number
	- **Flags:**  
		- 1-bit query/reply flag: 0 for query, 1 for reply
		- 1-bit authoritative flag: When DNS server is authoritative for hostname
		- 1-bit recursion-desired flag: If user desires DNS server to perform recursion if record not found
		- 1-bit recursion-available flag: if DNS server supports recursion
	- **Number of ... :** Number of occurrences of the four subsequent fields
- **Questions section:**
	- **Name:** name being queried
	- **Type:** Type A, Type MX, ...
- **Answer Section:**
	- Resource records for name queried
- **Authority Section:**
	- records of other authoritative servers
- **Additional:**
	- helpful records

## Inserting Records into DNS Database
- Given domain `networkutopia.com`
- Register domain name with **registrar** (ICANN accredited domain provider)
- Provide registrar with names and IP address of primary/secondary authoritative DNS servers
- Registrar inserts a Type NS and Type A resource records into TLD com servers
```
(networkutopia.com, dns1.networkutopia.com, NS)
(dns1.networkutopia.com, 212.212.212.1, A)
```
- Register Type A record for web server and Type MX record for mail server into authoritative DNS server on user side

- Done manually until recently, when an UPDATE option was added to DNS protocol to allow adding/deleting via DNS messages