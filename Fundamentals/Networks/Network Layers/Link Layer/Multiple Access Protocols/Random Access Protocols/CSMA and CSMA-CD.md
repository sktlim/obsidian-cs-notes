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
