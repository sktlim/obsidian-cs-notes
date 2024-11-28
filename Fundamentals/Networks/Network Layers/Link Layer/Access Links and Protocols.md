>**Point-to-point Link:** Single sender, single receiver
>**Broadcast Link:** multiple sending and receiving nodes connected to same single broadcast channel

## Multiple Access Protocol
- If more than 2 nodes transmit at once to a broadcast channel, transmitted frames collide at receivers and none of receiving nodes can make sense of what was transmitted

- A multiple access protocol should have the following characteristics:
	- When only one node has data to send, it sends it at a throughput of $R$ bps
	- If $M$ nodes have data to send, average throughput should be $R/M$ bps; However, this does not mean that each node always has instantaneous throughput of $R/m$ bps
	- Protocol decentralized (no master node)
	- Protocol simple (inexpensive to implement)

## Channel Partitioning Protocols
### TDM and FDM
- Time division multiplexing and frequency division multiplexing allows for partitioning of a broadcast channel's bandwidth. See [[Circuit Switching#Multiplexing in circuit-switches networks]] for more details
- TDM eliminates collisions and is completely fair, but
	- Each node is limited to average rate of $R/N$ bps even when its the only node with packets to send
	- Node must always wait for its turn even if its the only packet to be sent
- FDM avoids second disadvantage but shares the first one (that each node is limited in throughput)

### Code Division Multiple Access (CDMA)
- Assigns different codes to each node
- Each node uses code to encode the bits it sends
- CDMA has unique property of being decipherable as long as receiver knows the sender's code. See [[CDMA]]

## Random Access Protocols
- Transmitting node always transfers at full rate of channel
- When there is a collision, nodes that collided retransmit the frame after a random delay

### Slotted ALOHA
#### Initial Assumptions
- All frames consist of $L$ bits 
- Time is divided into slots of $L/R$ seconds
- Nodes start to transmit only at beginning of slots
- Nodes synchronized and know when each slot begins
- If 2 or more frames collide during a slot, all nodes detect collision event before slot ends

#### Operation
- When node has frame to send, it waits till beginning of slot and transmits entire frame in slot
	- If no collision, continue
	- If collide, node detects collision before end of slot, and retransmits its frame in each subsequent frame with probability $p$ until its frame gets retransmitted without a collision

#### Advantages
- Allows node to transmit continuously at full rate $R$ if its the only node
- Decentralized
- Simple 

#### Disadvantages
- If there are multiple active nodes, there are certain efficiency issues
	- Certain fraction of slots will have collisions and be wasted
	- Another fraction of slots will be empty due to probabilistic retransmission policy

**Successful Slot:** Slot in which exactly one node transmits 
**Efficiency** of slotted multiple access protocol defined to be long-run fraction of successful slots in the case of many active nodes, each always having a large number of frames to send

#### Maximum Efficiency of Slotted ALOHA
- **Assumptions**:
  - Each node **always has a frame to send**.
  - A node transmits with probability $p$ for:
    - A **fresh frame**.
    - A **frame that has already suffered a collision**.
  - There are $N$ nodes.
##### Derivation
1. **Probability of Success for a Given Slot**:
   - A slot is successful if **exactly one node transmits**, and the remaining nodes do not.
   - Probability that a given node transmits:  $p$
   - Probability that the remaining $N-1$ nodes do not transmit: $(1-p)^{N-1}$
   - Probability that a given node has a success: $p(1-p)^{N-1}$
2. **Probability That Any Node Has a Success**:
   - Since there are $N$ nodes, the probability that **any one of the $N$ nodes has a success** is:
$$Np(1-p)^{N-1}$$
3. **Efficiency of Slotted ALOHA**:
   - The **efficiency** is given by the probability of success for any node in a given slot:
$$\text{Efficiency} = Np(1-p)^{N-1}$$
##### Finding Maximum Efficiency
- To obtain the maximum efficiency for $N$ active nodes, we need to find the value of $p^*$ that maximizes the expression:$$Np(1-p)^{N-1}$$
To obtain max efficiency for large number of nodes, we take limit of above expression as $N$ tends to infinity

1. **Efficiency**: 
	- When a large number of nodes have many frames to transmit, only ($1/e=0.37$ )**37% of the slots** (at best) perform useful work. Effective transmission rate of the channel is: $$ \text{Effective Rate} = 0.37R \, \text{bps}$$
2. **Slot Utilization**: 
	- **37% of slots** are successfully used. 
	- **37% of slots** go **empty**. 
	- **26% of slots** result in **collisions**. 
3. **Practical Example**: 
	- A network administrator purchases a **100Mbps slotted ALOHA system**, expecting to achieve an aggregate data rate of 80Mbps. 
	- In reality, the channel is capable of transmitting frames at **100Mbps**. 
	- However, the **long-term successful throughput** will be less than 37MBps

### ALOHA Protocol
- When frame arrives, node immediately transmits frame in its entirety into its broadcast channel
	- If collision occurs, node immediately retransmits with probability $p$
	- Otherwise, node waits for frame transmission time, and then transmit frame with probability $p$, or wait for frame transmission time again with probability $1-p$

#### Maximum Efficiency of ALOHA

To determine the maximum efficiency of pure ALOHA, we focus on an **individual node**. The following assumptions are made, similar to the analysis of slotted ALOHA:

- The **frame transmission time** is taken as the **unit of time**.
- At any given time, the probability that a node is transmitting a frame is $p$.

For a frame that begins transmission at time $t_0$, no other node can begin its transmission during the interval $[t_0 - 1, t_0]$, as such a transmission would overlap with the beginning of the frame.
   - The probability that all other nodes do not begin a transmission in this interval is: $(1 - p)^{N-1}$

Similarly, no other node can begin a transmission during the interval $[t_0, t_0 + 1]$, as this would overlap with the latter part of the frame.
   - The probability that all other nodes do not begin a transmission in this interval: $(1 - p)^{N-1}$

Probability that a given node has a successful transmission: $p(1 - p)^{2(N-1)}$
   
By taking the limit as $N \to \infty$, and finding the optimal value of $p$ that maximizes the efficiency, the maximum efficiency of pure ALOHA is:  $$\text{Maximum Efficiency} = \frac{1}{2e} \approx 0.18$$ This is **half the efficiency** of slotted ALOHA, which achieves $\frac{1}{e} \approx 0.37$.

### Carrier Sense Multiple Access (CSMA)
> **Carrier Sensing:** Node listens to channel before transmitting. If frame from another node currently being transmitted into channel, node waits until it detects no transmission for some time before transmitting
> 
> **Collision Detection:** Transmitting node listens to channel while transmitting. If another node is transmitting an interfering frame, it stops transmitting and waits a random amount of time before repeating the cycle

![[CSMA.png|500]]
- Node B starts transmitting frame at time $t_0$ 
- Node D starts transmitting frame at time $t_1$. It doesn't sense frame from node B because it hasn't propagated to D yet, so D sense that frame is idle
- **Channel-propagation delay** of broadcast channel plays big part in determining performance
	- Longer this delay, the larger the chance that carrier-sensing node cannot sense another transmission in the network

### Carrier Sense Multiple Access with Collision Detection (CSMA/CD)
![[CSMACD.png|500]]
- This time, in CSMA with **collision detection (CD)**, channel can detect collisions
- Same thing happens initially, with B and D transmitting and $t_0$ and $t_1$ respectively
- D detects collision after transmitting for a while and aborts its transmission, while B aborts its transmission after a while when it detects it as well

#### CSMA/CD in Perspective of Network Adapter 
1) Network adapter receives datagram from network layer, prepares link layer frame, and puts it into the frame adapter buffer
	1) If adapter senses that channel is idle (no signal energy entering adapter), it starts to transmit the frame. 
	2) Else, it waits until there's no signal energy and starts transmitting the frame
2) While transmitting, adapter monitors for signal energy coming from other adapters using broadcast channel
	1) If adapter transmits whole frame without sensing signal energy, its done. 
	2) Else, it aborts the transmission and waits a **random amount of time** before returning to step 1.1/1.2 
### Binary Exponential Backoff Algorithm
>Solves problem of choosing optimal random time to wait if node aborts transmission

- Process
	- When transmitting frame that has $n$ collisions, node chooses value $K$ at random from $[0,2n-1]$
		- The more collisions experienced by frame, the larger the interval $K$ 

### Efficiency of CSMA/CD Protocol
> Long-run fraction of time during which frames are being transmitted on the channel without collisions when there is a large number of active nodes

$$\text{Efficiency} = \frac{1}{1 + 5 \cdot \frac{\text{Propagation Delay}}{\text{Transmission Time}}}$$
- Efficiency is high when $\alpha$ (the ratio of propagation delay to transmission time) is small, meaning shorter propagation times or longer frame sizes. 
- Efficiency decreases significantly as $\alpha$ increases, due to more time spent resolving collisions compared to transmitting useful data. 
- For practical Ethernet systems with small $\alpha$, CSMA/CD achieves high efficiency, often exceeding 90% under light traffic conditions.`

### Taking-Turns Protocol
- Polling protocol
	- Master node polls each node in round-robin manner, telling them they have x time to transmit
- Token-passing protocol
	- Passing around a token to signify that they can transmit