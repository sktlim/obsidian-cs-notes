> Congestion is caused by too many senders trying to send data at too high a rate

## Congestion Scenarios
### 2 senders and a router with infinite buffers
![[Congestion Scenario 1.png]]
- Host A and B sending data at $\lambda_{in}$ bytes/sec
- Packets from A and B pass through a router and over a shared outgoing link of capacity $R$
- Per-connection throughput capped at $R / 2$ even if sending rate above that
- As per-connection sending rate approaches $R / 2$, average delay approaches infinity due to large queueing delays

**Large queueing delays as packet-arrival rate approaches link capacity**

### 2 senders and router with finite buffers
![[Congestion scenario 2.png]]
- Assume connections are reliable
- $\lambda'_{in}$ is sometimes referred to as **offered load** to network

![[Scenario 2 performance analysis.png]]
1) A is able to (magically) determine whether buffers are free and only send when buffer is free, so loss is 0
	1) Ideal performance where $\lambda'_{in}$ scales linearly with $\lambda_{out}$
2) Sender retransmits only when packet is known for certain to be lost
	1) Rate of data delivery = $R/3 \rightarrow$ for each $0.5R$ delivered, $0.33R$ is original data and $0.167$ is retransmitted
3) Sender timeout prematurely and retransmits packet that just hasn't been ACKed
	1) Each packet forwarded twice $\rightarrow$ asymptotic throughput approaches $R/4$ as offered load approaches $R/2$

**Sender must retransmit to compensate for dropped packets due to buffer overflow**
**Unneeded retransmissions may cause router to use its link bandwidth to forward unneeded copies of a packet**

### 4 senders, routers with finite buffers, multihop paths
![[Congestion scenario 3.png]]
![[Scenario 3 performance analysis.png]]
- For low offered rate, no congestion so scales linearly with throughput
- When throughput exceeds capacity, 
	- packet drops lead to retransmissions, which add more traffic to network, which cause more packets to drop, which leads to more retransmissions, which leads to more traffic...
- Work done by first-hop router also wasted if packet is dropped at second-hop

**When a packet is dropped along a path, the transmission capacity that was used at each of the upstream links to forward that packet to the point at which it is dropped ends up having been wasted**

## Approaches to Congestion Control
- **End-to-End Congestion Control**
	- Network layer provides no explicit support to transport layer for congestion control purposes
	- Presence of network congestion must be inferred by end systems based on observed behavior 
	- **TCP uses this approach**. See more [[TCP Congestion Control]]
- **Network-assisted Congestion Control**
	- Routers provide explicit feedback on state of congestion
	- More recently, IP and TCP may also optionally implement network-assisted congestion control
	- Direct feedback may be sent from router to sender in the form of a choke packet
	- More common notification occurs when router mark/updates field in packet going from sender to receiver to indicate congestion. Receiver then informs sender of congestion