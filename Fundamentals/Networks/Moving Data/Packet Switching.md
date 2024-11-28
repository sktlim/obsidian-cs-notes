> Breaks data into packets and sends each packet on best available route

## Store-and-forward transmission
> Packet switch must receive entire packet before it can begin to transmit first bit of packet onto outbound link 
- Packet switch will have many incident links to switch incoming packet to outbound link
- Packet switch buffers packet bits before forwarding it onto outbound link
- Example
	- Assume no propagation delay and 1 packet
		- Source begins to transmit 1 packet at $t = 0$
		- At time $t= L/R$ seconds, source transmitted entire packet, and entire packet has been received and stored at packet switch 
			- Router can begin to transmit at $t= L/R$
		- At time $t= 2L/R$, router transmitted entire packet and packet received by destination
		- Total delay = $2L/R$
	- Assuming 3 packets,
		- Total delay = $4L/R$ 
	- Hence for $n$ links of rate $R$, end-to-end delay = $nL/R$
- Packet suffers **store-and-forward delays**
## Forwarding Tables and Routing Protocols
- Simple internet example
- Every end system has IP address
	- Source end system includes destination's IP address in packet header when it wants to send it ove
	- Each router has a **forwarding table** that maps destination addresses to that router's outbound links
	- When packet arrives at router, router examines destination address, searches forwarding table using destination address, finds appropriate outbound link, and directs packet to this link
- Analogous to car driver continuously asking for directions to find his destination
- See [[Forwarding Tables]] for more info

## Delay, Loss and Throughput
### Nodal Delay
![[Packet Switching Delay.png]]
> Total Nodal Delay = Nodal processing delay + queuing delay + transmission delay + propagation delay
#### Nodal Processing Delay
> Time required to examine packet's headers and determine where to direct packet to
- Typically on orders of microseconds or less
- Can include other factors, e.g. 
	- time needed to check for bit-level errors in packet that occurred during transmission
- Often negligible, but strongly influences router's throughput
#### Queuing Delay
> Queuing time in output buffer
- Each packet switch has multiple links attached
- For each attached link, packet switch has **output buffer/ queue**
	- Buffer stores packets router is about to send down that link
- If arriving packet needs to be transmitted on outbound link but sees link is busy, arriving packet waits in output buffer 
- Packet suffers output buffer **queuing delays**
- Typically microseconds to milliseconds
#### Transmission Delay
> Amount of time required to push all of packet's bits into link
- If length of packet is $L$ bits, and transmission rate is at $R$ bits/sec, transmission delay is $L / R$
- Typically microseconds to milliseconds
#### Propagation Delay
> Time required to propagate from beginning of link to router B
- bits propagates at propagation speed of link and is dependent on physical medium (equal to to a little less than speed of light)
- Propagation delay is **distance between 2 routers** divided by **propagation speed**
- Typically milliseconds in WANs

#### Difference between Transmission and Propagation Delay
- Transmission delay is function of packet's length and transmission rate of link, and has nothing to do with distance between routers
- Propagation delay is function of distance between routers and has nothing to do with packet length or transmission rate
- Analogy: transmission delay is a tollbooth processing a convoy, propagation delay is time to next toll booth

#### Queuing Delay and Packet Loss
- Important component of nodal delay
- Delay can vary from packet to packet, hence when characterizing $d_{queue}$, one typically uses statistical measures instead
- When is queuing delay important?
	- Let $a$ be average rate packets arrive in queue
	- $R$ is transmission rate (rate at which bits are pushed out of queue)
	- All packets contain $L$ bits
	- Average arrival rate (**Traffic Intensity**) = $aL / R$ 
		- if traffic intensity > 1, queue will increase without bound and delay will approach infinity as intensity approaches 1
		- **Design system so traffic intensity is never > 1**
		- Traffic intensity matters more in academic contexts and cannot fully characterize queue delay statistics
#### Packet Loss
> **Packet Loss:** dropped packets due to full buffer
- As amount of buffer space is finite, arriving packet may find buffer full
- **Packet loss** occurs as a result; either arriving packet or already-queued packets will be dropped
- Packet delays therefore don't really hit infinity as well, since queue space is finite
- Performance at node is hence also measured by **probability of packet loss**

### End-to-End Delay
> Total delay from source to destination

$$delay_{end-end} = N(d_{process}+d_{transmission}+d_{propagation})$$
where there are $N-1$ routers
- Assumptions
	- Network is uncongested ($d_{queue}$ negligible)

### Throughput 
- **Instantaneous throughput:** Rate at which destination host is receiving file at that instance of time
- **Average Throughput:** Number of bits transferred divided by time taken
	- If file consists $F$ bits, and transfer takes $T$ seconds, average throughput is $F/T$ bits/sec
- **Bottleneck Link:** Throughput is limited to the minimum rate among the links connecting host to destination
	- Given $N$ links between server and client, with transmission rates of each link being $R_{1}, R_{2}, \dots ,R_{N}$, throughput is $min{\{R_{1}, R_{2}, \dots ,R_{N}\}}$
	- Approximation works assuming no intervening traffic
- More generally throughput also depends on intervening traffic; Link with high transmission rate may still be bottleneck link if link is saturated with other traffic