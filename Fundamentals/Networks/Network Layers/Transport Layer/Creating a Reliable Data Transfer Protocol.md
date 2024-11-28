## Assumptions 
- Packets delivered in order they were sent, with some packets being lost
- Sending side of data transfer protocol will be invoked by `rdt_send()`
- On receiving side, `rdt_rcv()` will be called when packets arrives
- When `rdt` wants to deliver data to upper layer, it will call `deliver_data()`
- Only **unidirectional data transfer** is considered (transfer from sending to receiving side)

Send/Receive models are referred to by **Finite State Machines (FSM)**
- Event causing transition is shown by text above horizontal
- Actions taken below horizontal
- symbol Λ below explicitly denotes the lack of an action or event (depending on its position above/below horizontal)
- Initial state indicated by dashed arrow
## `rdt1.0`: Reliable data over perfectly reliable channel
![[rdt1.0 send.png|350]]![[rdt1.0 receive.png|345]]
### Send
- Accepts data from upper layer through `rdt_send(data)` event
- Creates and sends packet through `udt_send(packet)`

### Receive
- Accepts packet from sender through `rdt_rcv(packet)`
- Extracts and delivers data to upper layer recipient through `deliver_data(data)`

### Model Evaluation
- No difference between packet and data
- As channel is perfectly reliable, receiving side will never report errors

## `rdt2.0`: Reliable data transfer over channel with bit errors
- Bits in packet might be corrupted during delivery
- Simple way to resolve is using **positive acknowledgements (ACK)** and **negative acknowledgements (NAK)** for receiver to let sender know if message was received correctly
- If message was not received correctly (NAK), sender retransmits it
- Reliable data transfer protocols based on retransmission are known as **Automatic Repeat reQuest (ARQ) protocols** and require 3 additional capabilities
	- **Error Detection:** Receiver must be able to detect when bit errors occurred
	- **Receiver Feedback:** ACK and NAK packets
	- **Retransmission:** Packet received in error will be retransmitted by receiver

![[rdt2 send.png]]![[rdt2 receive.png]]

### Send 
- **Left State:** Wait for data from upper layer
	- Sends packet of data and checksum to recipient and transits to right state
- **Right State:** Wait for ACK or NAK from receiving side
	- Retries sending packet if NAK received
	- Transitions back to left state if ACK received 
### Receive
- Packet corrupted: `rdt_rcv(rcvpckt) && corrupt(rcvpckt)`
	- Sends NAK over to sender
- Packet ok: `rdt_rcv(rcvpckt) && notcorrupt(rcvpckt)`
	- Extracts and delivers data to upper layer and sends ACK

### Model Evaluation
- **Issue:** ACK/ NAK can be corrupted
- Minimally, checksum bits have to be added to ACK/NAK
- 3 possible solutions
	- **Adding a New Message Type**: Introducing a "What did you say?" packet for clarification can lead to confusion and endless garbled exchanges.
	- **Error Correction with Checksums**: Using sufficient checksum bits allows the sender to detect and recover from bit errors but requires a reliable channel without packet loss.
	- **Resending Packets**: Retransmitting on garbled ACK/NAK introduces **duplicates packets**, making it hard for the receiver to distinguish new data from retransmissions.
- Simple solution is for sender to number its packets and add a new **sequence number** field
- Receiver just needs to check this field to determine whether its a retransmission

## `rdt2.1`: Sequence numbers (SNs) to resolve corrupted ACK/NAK
### Send 
![[rdt2.1 send.png|]]

- Wait for call 0 (0 is sequence number) from above
	- When upper layer sends data, FSM makes packet with 0, data and checksum and sends packet to recipient and transits
- Wait for ACK or NAK 0
	- If packet is corrupt or NAK is received from recipient, retry sending packet
	- Else, transit
- Wait for call SN 1 from above
	- Repeat above 2 steps and transit back to 0 (model assumes only 2 packets)

### Receive
![[rdt2.1 receive.png]]
- Wait for 0 from below
	- If packet corrupt, send NAK and checksum for NAK to sender
	- If packet not corrupt and has SN1 (`has_seq1(recvpkt)`), sends ACK and checksum to sender 
	- If packet not corrupt and has SN0 (`has_seq0(recvpkt)`), delivers data to upper layer, sends ACK and checksum to sender, and transits to next state
- Wait for 1 from below
	- If packet corrupt, send NAK and checksum for NAK to sender
	- If packet not corrupt and has SN0 (`has_seq0(recvpkt)`), sends ACK and checksum to sender 
	- If packet not corrupt and has SN1 (`has_seq1(recvpkt)`), delivers data to upper layer, sends ACK and checksum to sender, and transits to next state

## `rdt2.2`: NAK free reliable data transfer
![[rdt2.2 send.png]]
![[rdt2.2 receive.png]]

## `rdt3.0`: Reliable data transfer over lossy channel with bit errors
- Underlying channel can lose packets as well
- Need to address
	- How to detect packet loss?
		- New protocol mechanism
	- What to do when packet loss occurs?
		- Check summing, sequence numbers, ACK packets

- Textbooks puts responsibility of detecting and recovering lost packets on sender
- If sender waits long enough such that its certain packet is lost, it can simply retransmit the packet
- Sender must wait at least as long as a round-trip delay, but this is hard to estimate
- **Solution**: Anyhow choose a good enough time

- Implementing a time based retransmission requires **countdown timer** that can interrupt sender after given amount of time has expired
- Sender will need to be able to 
	1) Start timer each time packet is sent
	2) Respond to timer interrupt
	3) Stop timer

![[rdt3 sender.png]]
- Sometimes known as alternating bit protocol as SN alternates between 0 and 1

## Summary
- `rdt1.0`: Simple barebone models for RDT over perfectly reliable channel with no loss no bit errors; Akin to UDP
- `rdt2.0`: RDT over reliable channel with bit errors
	- Introduce **ACK/NAK** with checksums for data to spot bit errors
	- ACK/NAK might have bit errors so we introduce **Sequence Numbers** so that in that event, sender can simply resend
		- This is better because ?
- `rdt3.0`: RDT over lossy channel with bit errors
	- If packets lost, we can detect with sequence numbers, checksums, ACK/NAK
	- To detect packet loss, we introduce **countdown timers** that will allow sender to resend packet if no response for certain amount of time

## `rdt3.0` Performance
- Essentially a stop-and-wait protocol (have to stop operations and wait for response before sending next thing)
![[rdt3 operation with no loss.png|346]]![[rdt3 operation with lost packet.png|350]]
![[rdt3 with los ack.png|345]]![[rdt3 premature timeout.png|350]]
- If there are 2 end systems with RTT 30ms, connected by channel with transmission rate $R$ = 1Gbps, transmitting packets of size $L$ =1000 bytes
$$d_{transmission} = \frac{L}{R} = \frac{8000}{1*10^9} = 8microsec onds$$
- If sender sends packet at $t=0$, last bit enters at $t=8ms$, which means that receiver receives last bit of packet at $t=\frac{RTT}{2} + d_{transmission}=15.008ms$
- Assuming ACK for packet is small, sender receives ACK at $t=30.008ms$, of which sender was only sending for 0.008ms
- $Sender \space Utilization \space U_{s}=\frac{0.008}{30.008}= 0.00027$
- Effective throughput of 267kbps, despite 1Gbps link
- **Solution:** Use pipelining

## Pipelined protocol
![[rdt3 stop-and-wait.png]]
![[rdt3 pipeline.png]]
- Consequences of pipelining
	- **Range of sequence numbers must be increased** as each in-transit packet needs to have their own sequence number
	- **Sender and receiver needs to buffer more than 1 packet**
- Pipelined error recovery has 2 approaches to account for the above consequences
	- [[Go-Back-N (GBN)]]
	- [[Selective Repeat (SR)]]
