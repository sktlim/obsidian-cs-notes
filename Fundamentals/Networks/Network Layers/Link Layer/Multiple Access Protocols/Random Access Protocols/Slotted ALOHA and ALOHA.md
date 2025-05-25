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
