## IPv6 Datagram Format
![[IPv6 Datagram Format.png]]
- **Expanded Addressing Capabilities**: Increases size of IP address from 32 bits to 128 bits
	- Now, every grain of sand is addressable
	- In addition to unicast and multicast, IPv6 introduces a **anycast address**, which allows datagram to be delivered to any one of a group of hosts
- **Streamlined 40-byte header**
- **Flow labeling:** Labeling of packets belonging to particular flows that sender requires special handling 

### Fields
- **Version (4-bits)**
	- IPv6 carries a value of 6; Changing this to 4 doesn't make it a valid IPv4 datagram
- **Traffic Class (8-bits)**
	- Can be used to give priority to certain datagrams within a flow
- **Flow Label (20-bits)**: Used to identify flow of datagrams
- **Payload Length (16-bits)**: Unsigned integer giving number of bytes in payload
- **Next Header (8-bits)**: Identifies protocol to which contents of datagram will be delivered; Uses same values as protocol field in IPv4 header
- **Hop Limit (8-bits)**: Contents decremented by 1 for each router that forwards datagram; If 0, datagram discarded
- **Source and destination addresses**
- **Data**

### Missing fields from IPv4
- **Fragmentation/reassembly fields**: IPv6 does not allow for fragmentation and reassembly at routers; these operations can only be done at source/destination hosts
	- If IPv6 datagram too big to be forwarded, router simply drops datagram and sends a "Packet Too Big" ICMP message back to sender
	- Removing functionality speeds up IP forwarding within network
- **Header Checksum**: Transport and link layer protocols do checksum, so designers felt it was redundant. Removed to improve performance of IP forwarding
- **Options:** Not part of header, but is one of the possible next headers pointed to from within IPv6 header 

## IPv4 to IPv6 Transition
### Tunneling
![[Tunneling.png|500]]
- IPv6 nodes at end of tunnel take **entire** IPv6 datagram and puts it in payload of IPv4 datagram
- IPv4 datagram then addressed to IPv6 node at other end of tunnel
- IPc6 on end of tunnel takes IPv4 datagram and realizes it contains an IPv6 datagram by observing protocol number field in IPv4 is 41, extract the IPv6 datagram, and continues routing it to destination