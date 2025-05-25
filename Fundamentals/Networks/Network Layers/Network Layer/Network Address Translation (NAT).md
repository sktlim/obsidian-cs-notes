> Modifies IP address information in packet headers as they pass through a router, allowing multiple devices on a **private network** to share a single public IP address. 
> 
> Network Address Translation (NAT) is commonly used for conserving IPv4 addresses and providing a layer of security by masking internal IPs.

![[NAT.png]]

#### How it works
- NAT-enabled router behaves like a single device with single IP address to outside world
- Wrt diagram, all network entering and leaving router has IP address 138.76.29.7
- NAT Routers accomplish this using the **NAT Translation Table** and to include IP and port numbers as entries
- Example illustrated in diagram
	- Host 10.0.0.1 requests web page on IP `128.119.40.186` at port `80`
	- Host assigns arbitrary source port number `3345` and sends datagram into LAN
	- NAT router receives datagram, generates new source port number `5001` and replaces source port number, replaces source IP with its IP `138.76.29.7`
		- NAT can select any source port number not in NAT translation table, meaning that NAT protocol can simultaneously support 60,000 connections with 16-bits source port number
	- When packet arrives from web server, router indexes NAT translation table and delivers packet to requesting host

#### Issues raised by NAT
- Port numbers should be used for addressing processes and not addressing hosts
	- server processes wait for incoming requests at well-known port numbers 
	- Technical solutions involve **NAT traversal tools** and **Universal Plug and Play (UPnP)**, which allows host to discover and configure nearby NAT
- NAT violates principle that hosts should be talking directly to each other
	- Servers as a **middlebox**, network device that does more than standard packet forwarding