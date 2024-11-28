> **Data Plane**: Handles real-time **packet forwarding** based on precomputed rules from the control plane. It operates at the hardware level (e.g., ASICs in routers/switches) to ensure high-speed, low-latency processing. Examples include applying ACLs, NAT, and IP lookups in routing tables.

> **Control Plane**: Manages the **decision-making logic**, such as building routing tables and configuring forwarding rules. It runs on the device's CPU or in centralized SDN controllers and uses protocols like OSPF and BGP to calculate paths and respond to topology changes.

## Primary Role of Network Layer
Move packets from sending host to receiving host. Done through
- **Forwarding:** router-local action of transferring a packet from an input link interface to the appropriate output link interface
- **Routing:** network-wide process that determines the end-to-end paths that packets take from source to destination

### Forwarding Table
- Key element in every router
- Values in arriving packet's header indexes into the forwarding table, and values stored in forwarding table entry indicate outgoing link interface to which packet is to be forwarded

## Control Plane: Software-Defined Network (SDN) Approach
- Physically separate **remote controller** sends forwarding tables to each router
- Routing device performs forwarding only, while remote controller is responsible for computing and distributing forwarding tables
- Remote controller might be implemented in data center and managed by ISP

## Network Service Model
> Defines characteristics of end-to-end delivery of packets between sending and receiving hosts

Services could include:
- Guaranteed delivery
- Guaranteed delivery with bounded delay
- In-order packet delivery
- Guaranteed minimal bandwidth
- Security

Internet service model provides only 1 guarantee: **best-effort service**
- Practically promises nothing; a network delivering no packets fit this definition