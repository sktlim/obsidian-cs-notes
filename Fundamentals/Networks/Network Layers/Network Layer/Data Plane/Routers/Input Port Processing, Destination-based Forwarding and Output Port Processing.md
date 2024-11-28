![[Input Port Processing.png]]
- At each line card, a shadow copy of the forwarding table exists (copied over from routing processor)
- Forwarding decisions can therefore be made locally at each input port and avoid a processor bottleneck 

## Processing Example
- Uses the **match plus action** abstraction pattern
- For 32-bit IP addresses, as storing 4 billion addresses (1 entry for each address) in the forwarding table is obviously not good, a prefix table is used instead

| **Prefix**                | **Link Interface** |
| ------------------------- | ------------------ |
| 11001000 00010111 00010   | 0                  |
| 11001000 00010111 0001100 | 1                  |
| 11001000 00010111 00011   | 2                  |
| Otherwise                 | 3                  |
- Router matches prefix of **destination address** with table and forward to relevant outgoing link
- When there are multiple matches, router uses **longest prefix matching rule**
	- **longest prefix matching rule**: Find longest matching prefix and forward packet to link interface associated with that prefix
- Once packet's output port determined, packet can be sent into switching fabric
	- Packet may be blocked and queued in some designs if other ports are using the switch fabric
### Real-life implementations
- Due to requirement for nanosecond speeds, fast lookup algorithms are used (instead of linear search)
- Memory access times are also a concern, resulting in designs with embedded on-chip DRAM and SRAM 
- In practice, [[Ternary Content Addressable Memories (TCAM)]] are often used for lookup

## Output Port Processing
![[Output Port Processing.png]]
- Takes packets in output port's buffer and transmits them over outgoing link

## Queueing
- Location and extent of queueing depends on traffic load, relative speed of switching fabric, and line speed
- Given
	- $N$ input ports, $N$ output ports
	- input/output transmission rate = $R_{line}$
	- Time to send packet on any link = Time to receive packet on any link
	- Switching fabric rate = $R_{switch}$ 

	- If $R_{switch} = N*R_{line}$, only negligible queuing will occur at input ports

### Input Queueing
- If switch fabric not fast enough, packet queueing occurs at input ports
- This causes **head-of-line (HOL) blocking**, whereby a queued packet in an input queue must wait for transfer through fabric because its blocked by another packet at head-of-line
![[HOL blocking.png|400]]
- 2 packets at front of 2 different input queues destined for same upper-right output port
- Switch fabric chooses to transfer from front of input port 1 queue
	- Packet behind front of input port 1 queue has to wait
	- Packet in front of input port 3 queue has to wait
	- Lightly shaded packet in port 3 queue also has to wait, despite there being no contention for output port 2
- Once packet arrival rate hits 58% capacity, input queue (under certain assumptions) will grow to unbounded length

#### Solutions to HOL Blocking

### Output Queueing
![[output port queueing.png|400]]
- Given $R_{switch} = N*R_{line}$ and packets arriving at each of the $N$ port is destined for same output port
- In time that $N$ new packets arrive at port, only 1 is being sent out and the $N$ arriving packets have to queue 
- When not enough memory to buffer incoming packet, decision must be made to drop arriving packet (**drop-tail policy**) or to remove already queued packets
	- May be advantageous to drop/mark header of packet before queue is full to signal congestion to sender
	- Known as part of **active-queue management (AQM) algorithms**
	- One such popular algorithm is **Random Early Detection**
- With multiple packets at each port, **packet scheduler** then picks one packet to transmit

### Buffer Sizing 
- Rule of thumb is $$B=RTT*C$$
- where 
	- $B$ = buffer size
	- $RTT$ = round-trip time
	- $C$ = link capacity
