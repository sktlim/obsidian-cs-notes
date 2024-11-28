## IPv4 Datagram Structure
![[IPv4 Datagram format.png]]
- **Version number (4-bits)**: Specify IP protocol version. Different versions use different formats
- **Header Length (4-bits):** Needed to determine where in IP datagram payload begins
	- Typical IP datagram has 20-byte header 
- **Type of Service (8-bits):** Allow different IP datagrams to be distinguished
	- Might be useful to distinguish real-time datagrams from non-real-time traffic
	- 2 bits also used for ECN
- **Datagram Length (16-bits):** Total length of datagram
	- Max theoretical size = 65,535 bytes, but rarely larger than 1500 bytes (allows datagram to fit in maximally sized Ethernet frame)
- **Identifier (16-bits), Flags (3-bits), and Fragmentation Offset (13-bits)**: For IP fragmentation
- **Time-To-Live (TTL) (8-bits):** Ensures datagram does not circulate forever in network. Field decremented by 1 each time datagram processed by router. If 0, router drops datagram
- **Protocol (8-bits):** Typically only used when TCP reaches final destination
	- Value indicates specific transport-layer protocol to which data portion of IP should be passed (e.g. 6 is TCP, 17 is UDP)
	- Analogous role to port number field in transport layer segment
- **Header Checksum (16-bits):** Aids router in detecting bit errors
	- Computed by treating each 2 bytes in header as a number and summing these using 1s complement; Router computes checksum from header and verifies against checksum and detects error if they don't equal
	- Routers typically discard datagrams with errors
	- Checksum must be recomputed at every router as some fields (e.g. TTL) will change
- **Source and destination IP addresses (32-bits each):** Source host determines IP address using DNS lookup
- **Options (32-bits):** Allows headers to be extended but causes datagram to have variable lengths, so one cannot determine where data starts before examining the datagram
	- Excluded in IPv6
- **Data:** Contains transport-layer segment to be delivered

IPv4 datagram contains 20-bytes of header without options

## IPv4 Datagram Fragmentation
![[IPv4 fragmentation and assembly.png]]
- Not all link-layer frames can carry network-layer packets of same size
	- Ethernet frames carry 1500 bytes, while some wide-area links only carry up to 576 bytes
- **Maximum Transmission Unit (MTU):** Max amount of data a link-layer frame can carry
- MTU of link-layer protocol places hard limit on datagram size
- **Solution:** Fragment payload into 2 or smaller datagrams and uses Identifier (16-bits), Flags (3-bits), and Fragmentation Offset for reassembly by IP at end system

- When datagram is created, sending host stamps datagram with **identification number, source address, and destination address** 
	- Host increments identification number for each datagram it sends
- When router needs to fragment datagram, each resulting datagram is stamped with source address, destination address, and identification number of original datagram
- End system can check source address and identification number to piece datagram back together

- **Flag Bit:** Destination host is sure that it received last fragment when the last fragment has a flag bit set to 0 (all other fragments set to 1)
- **Offset:** Used to determine where fragment fits in original IP datagram

## IPv4 Addressing
- Boundary between host and physical link is known as **interface** 
	- Router has multiple interfaces, while host has 1
- Every host and router can send datagrams, so IP requires each host and router to have their own IP address
- IP address is technically associated with the interface, rather than the host/router containing the interface

- Each IPv4 address is 32-bits long, meaning there are approximately 4 billion IPv4 addresses
	- Written in dotted-decimal notation
	- Globally unique (except for NATs)
	- Portion of IP address determined by subnet its connected to
### Subnet and subnet masking
![[Interface address and subnets.png|400]]
- Upper-left side have addresses $223.1.1.xxx$ (same leftmost 24 bits)
	- Interfaces are connected by network without routers (can be by Ethernet switch or wireless access points)
- Network connecting 3 host interfaces and 1 router interface is known as a **subnet**
- IP addressing assigns address: 223.1.1.0/24 ("/24" is **subnet mask**, indicate leftmost 24 bits which defines subnet range)

>To determine the subnets, detach each interface from its host or router, creating islands of isolated networks, with interfaces terminating the end points of the isolated networks. Each of these isolated networks is called a **subnet**. ("Cloud" connecting end points)

### Classless InterDomain Routing (CIDR)
Internet's address assignment strategy is known as **Classless InterDomain Routing (CIDR)** 
- IP address divided into 2 parts and has dotted decimal form with slash $a.b.c.d/x$
- $x$ most significant bits are often referred to as **prefix** of address 
- Only $x$ bits are considered outside an organization's network
- Remaining bits (32 - $x$) are used to distinguish device in network

**Classful Addressing:** Before CIDR, network portions of IP constrained to 8, 16, 24 bits as subnets were known as Class A, Class B, Class C respectively for each sized subnet
- Class C can accommodate $2^8-2 = 256$ hosts (last 8 bits - 2 for special use)
- Class B can accommodate $2^{16}-2 = 65,643$ hosts
Class B rapidly depleted as a result, because orgs needed more than 256 addresses

### How CIDR facilitates routing
- If ISP advertises IP address block $a.b.c.d/x$, rest of world no need to know what's inside the address block
- Ability to advertise multiple networks referred to as **address aggregation**

### IP Broadcasting
- Address 255.255.255.255
- Message delivered to all hosts on same subnet
- Routers optionally forward message into neighboring subnets (but usually don't)

### How to obtain an address block
- To obtain block for use within organization's subnet, network admin checks with ISP, who then allocates a block from their own address block
- ISP gets its block from ICANN 
