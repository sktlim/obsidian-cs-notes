> Network-layer protocol used by routers, hosts, and other devices to send control and error messages. 

## Properties
- Often considered part of IP, but architecturally lies above IP as ICMP messages carried inside IP datagrams as payload
###  Packet Format
Each ICMP packet is encapsulated in an IP packet. Its structure includes:

| **Field**                 | **Description**                                                                                             |
| ------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Type (8 bits)**         | Identifies the ICMP message type (e.g., 0 for Echo Reply, 3 for Destination Unreachable).                   |
| **Code (8 bits)**         | Provides additional context for the message type (e.g., reason for "destination unreachable").              |
| **Checksum (16 bits)**    | Verifies the integrity of the ICMP message.                                                                 |
| **Message-Specific Data** | May include the original IP header, additional IP data, or other information specific to the type and code. |
## Well-known Uses
#### `ping`
- **Purpose**:
    - Ping checks network connectivity and measures the round-trip time (RTT) between a source and destination.
- **Mechanism**:
    - The source sends **ICMP Echo Request** messages (Type 8, Code 0) to the destination.
    - Each Echo Request includes:
        - A sequence number to track responses.
        - A timestamp to measure RTT.
- **Destination's Role**:
    - When the destination receives an Echo Request:
        - It replies with an **ICMP Echo Reply** message (Type 0, Code 0).
        - The reply contains the same sequence number and timestamp as the request.
- **Round-Trip Time (RTT)**:
    - When the source receives the Echo Reply:
        - It calculates RTT as the time difference between sending the request and receiving the reply.
        - It matches the reply to the original request using the sequence number.
- **Output**:
    - Ping outputs:
        - The destination's IP address.
        - RTT for each packet.
        - Packet loss statistics (if any requests fail).
        - Summary of minimum, maximum, and average RTTs.
- **Stopping Condition**:
    - Ping continues sending Echo Requests either:
        - For a specified number of packets.
        - Until manually stopped (default behavior on many systems).
- **Requirements**:
    - The Ping program must:
        - Generate ICMP Echo Request messages.
        - Be notified when ICMP Echo Replies are received by the operating system.

####  `tracert`
- **Purpose**:
    - Traceroute identifies the number and identities of routers between a source and a destination, as well as round-trip times (RTTs) for each hop.
- **Mechanism**:
    - Source sends UDP datagrams with an unlikely port number.
    - Each datagram is sent with a progressively incremented **TTL**:
        - First datagram: TTL = 1, second: TTL = 2, third: TTL = 3, and so on.
    - Source starts a timer for each datagram sent.
- **Router's Role**:
    - When a datagram reaches a router and the TTL expires:
        - The router discards the datagram.
        - It sends an **ICMP Time Exceeded** message (Type 11, Code 0) back to the source.
        - The ICMP message contains the router’s IP address and name.
- **Round-Trip Time (RTT)**:
    - When the source receives the ICMP message, it:
        - Stops the timer to calculate the RTT for that hop.
        - Extracts the router’s name and IP address from the ICMP message.
- **Stopping Condition**:
    - When the datagram reaches the destination:
        - The destination host discards it due to the unlikely UDP port number.
        - It sends an **ICMP Port Unreachable** message (Type 3, Code 3) back to the source.
        - Upon receiving this message, the source stops sending further datagrams.
- **Output**:
    - Traceroute outputs the names, IP addresses, and RTTs for each hop.
    - Typically, Traceroute sends **three probes per TTL**, so it provides three RTT measurements per hop.
- **Requirements**:
    - The Traceroute program must:
        - Generate UDP packets with specific TTL values.
        - Be notified when ICMP messages are received by the operating system.

####  Router Discovery:
- Uses **Router Advertisement (Type 9)** and **Router Solicitation (Type 10)** for host-router interaction.
#### Path Optimization:
- Employs **Redirect (Type 5)** messages to suggest a more efficient route to a destination.


## Common ICMP Messages
#### 1. Error Messages

|**Message**|**Type**|**Code**|**Description**|
|---|---|---|---|
|**Destination Unreachable**|3|0: Network unreachable|Indicates the destination network is unreachable.|
|||1: Host unreachable|Indicates the destination host is unreachable.|
|||2: Protocol unreachable|The protocol in the IP header is unsupported.|
|||3: Port unreachable|The destination port is closed or unavailable.|
|||9-15: Other reasons|Includes administrative filtering or rejected communication.|
|**Time Exceeded**|11|0: TTL expired|Packet exceeded its time-to-live (TTL) in transit, often used in traceroute.|
|||1: Fragment reassembly timeout|The fragments of a datagram failed to reassemble within the allotted time.|
|**Parameter Problem**|12|0: Pointer indicates error|An issue occurred with an invalid IP header field.|
|||1: Missing required option|Required options in the header are missing.|
|**Source Quench**|4|Deprecated|Used to indicate congestion but is now deprecated due to inefficiency.|

#### 2. Query Messages

| **Message**              | **Type** | **Code**                | **Description**                                                               |
| ------------------------ | -------- | ----------------------- | ----------------------------------------------------------------------------- |
| **Echo Request**         | 8        | 0                       | Sent by tools like `ping` to test connectivity.                               |
| **Echo Reply**           | 0        | 0                       | Reply to an Echo Request, indicating successful delivery and round-trip time. |
| **Router Advertisement** | 9        | 0                       | Routers advertise their presence and network parameters.                      |
| **Router Solicitation**  | 10       | 0                       | Hosts request information from routers.                                       |
| **Timestamp Request**    | 13       | 0                       | Requests time synchronization information.                                    |
| **Timestamp Reply**      | 14       | 0                       | Provides a response to a Timestamp Request with time synchronization data.    |
| **Address Mask Request** | 17       | 0                       | Asks for the subnet mask configuration from a router.                         |
| **Address Mask Reply**   | 18       | 0                       | Returns the subnet mask information requested by a host.                      |
| **Redirect**             | 5        | 0: Redirect for network | Suggests a better route to the destination.                                   |
|                          |          | 1: Redirect for host    | Indicates a specific host route is better.                                    |