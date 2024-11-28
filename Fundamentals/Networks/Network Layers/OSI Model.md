> **Service Model:** Services offered to layer above

Each layer performs certain actions within that layer using services of layer directly below

## 5 layer Internet Protocol Stack
1) **Physical**
	1) Move individual bits within frame from one node to next
	2) Protocols link dependent and further depend on actual transmission medium
2) **Data Link**
	1) To move packet from one node to the next in the route; network layer passes datagram to link layer, which sends to link layer in destination
	2) May be handled by different link layer protocols
	3) Includes
		1) Ethernet
		2) Wifi
		3) DOCSIS
	4) Packet in this layer referred to as **frames**
3) **Network**
	1) Moving network-layer packets from one host to another
	2) Transport layer in source passes a transport layer segment and destination address to network layer, which then delivers segment to network layer of destination
	3) Includes
		1) IP: Defines fields in datagram 
	4) Packets in this layer referred to as a **datagram**
4) **Transport**
	1) Transports app layer messages between app endpoints
	2) Includes
		1) TCP
		2) UDP
	3) Packet in this layer referred to as a **segment**
5) **Application**
	1) Where network apps and app-layer protocol reside
	2) Includes
		1) HTTP
		2) SMTP
		3) FTP
		4) DNS
	3) Distributed over multiple end systems
	4) Packet in this layer referred to as a **message**

## OSI Model
- 5 layer model + session layer + presentation layer 
- **Presentation Layer:** Provide services that allow communicating apps to interpret meaning of data exchanged
	- e.g. data compression, data encryption, data description
- **Session Layer:** Provides for delimiting and synchronization of data exchange, including means to build checkpointing and recovery scheme
- Not present in internet model because decision to build these services is up to app developer

## Encapsulation
![[IP stack diagram.png]]
- At sending host, an app-layer message is passed to transport layer
- Transport layer appends transport layer header info $H_{t}$ that will be used in receiver side
- App-layer message and transport-layer header constitutes the **transport-layer segment** 
- Transport layer then passes segment to network layer, which adds network layer info $H_{n}$ such as source and destination info, creating a **network-layer datagram**
- Datagram is passed to link layer which adds its own header info and creates **link-layer frame**
- At each layer, packet has 2 types of fields
	- **Header fields:** header info added by current layer
	- **Payload field:** Packet from layer above
