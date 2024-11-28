> **Transport Layer Protocol** provides for logical communication between app processes running on different hosts

**Logical Communication:** As if hosts were directly connected

Transport Layer packet is known as a **segment.** However, internet literature refers to UDP packets as datagrams and TCP packets as segments. Transport Layer builds on services provided by the Network Layer, mainly the [[Internet Protcol (IP)]]
## Internet Transport Layer protocols
- [[User Datagram Protocol (UDP)]]: unreliable, connectionless service
- [[Transmission Control Protocol (TCP)]]: reliable, connection-oriented service

### Fundamental Responsibilities of UDP and TCP
- Extend IP's delivery service on 2 end systems to a delivery service for 2 processes on each end systems. This is known as **transport-layer multiplexing/ demultiplexing**
- Provide integrity checks by including error detection fields in headers 

#### Additional Services provided by TCP
- Reliable data transfer
- Congestion control