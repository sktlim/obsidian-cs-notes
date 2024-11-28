> Avoids unnecessary transmissions by having sender retransmit only packets suspected to be received in error 

## Key Idea
- Flaw with [[Go-Back-N (GBN)]] is that when window size and bandwidth-delay product are large, many packets might be in pipeline and single packet error will require large number of files to be retransmitted
- SR solves this by only retransmitting erroneous packets, and would require receiver to individually acknowledge correctly received packets
- SR receiver will ACK a correct packet whether or not its in order
	- Out of order packets are buffered until any missing packets are received, at which point batch of packets can be delivered in order to upper layer

![[Selective Repeat example.png]]
[Why are there not yet ACKed packets at send_base?]:    
## SR Sender
- **Data received from above**:
    - The sender checks if the sequence number is within the sender’s window:
        - If yes, the data is packetized and sent.
        - If not, the data is buffered or returned to the upper layer for later transmission.
- **Timeout**:
    - Timers are used to handle lost packets.
    - Each packet has its own logical timer.
    - On timeout, the corresponding packet is retransmitted.
    - A single hardware timer can mimic multiple logical timers.
- **ACK received**:
	- When an ACK is received:
	- If the sequence number in the ACK equals `send_base`:
	    - The packet is marked as acknowledged.
	    - The window base (`send_base`) is moved forward to the next unacknowledged packet.
	    - If new packets now fall within the window, they are transmitted.
	- If the ACK is for a packet outside the sender's current window, it is ignored.

## SR Receiver
- **Packet with sequence number in `[rcv_base, rcv_base+N-1]` is correctly received**:
    - If the packet has not been previously received, it is buffered.
    - If the packet has a sequence number equal to `rcv_base`:
        - Deliver the packet, along with any consecutively buffered packets, to the upper layer.
        - Move the receive window forward by the number of packets delivered to upper layer.
    - A selective ACK (SACK) is returned to the sender.
- **Packet with sequence number in `[rcv_base-N, rcv_base-1]` is correctly received**:
    - If the packet has already been acknowledged, another ACK is generated for it.
    - Receiver re-acknowledges already received packets with certain SNs below current window base
	    - If there is no ACK for packet `send_base`, sender will eventually retransmit `send_base` even though receiver has received. Sender's window will also never move forward
- **Otherwise**:
    - The packet is ignored.
### Why ACKing packets before window is important (point 2)
- If sender and receiver windows go out of sync, important to re-ACK so that transmission can move forward
	- Imagine if sender sends 3 packets, receiver receives and sends 3 ACKs, but all 3 ACKs lost
	- Receiver window moves forward, but sender's window stuck
## Example SR Operation
![[SR Operation.png]]

## Issue with Finite Window Size
> New Packet or Retransmission?

![[finite window issue1.png]]
![[finite window issue.png]]
- Figure a
	- Window moves forward to next cycle for receiver but not for sender
	- Sender resends packet 0 with already sent info but receiver treats it as new info when its a **retransmission**
- Figure b
	- Receiver window crosses boundary to next cycle, and receives packet 0 as **new info**
- Scenarios identical from receiver viewpoint
- **Key Question:** How small should window size be? 
	- **Answer:** $size_{window} \leq \frac{1}{2}*size_{SN\space space}$

