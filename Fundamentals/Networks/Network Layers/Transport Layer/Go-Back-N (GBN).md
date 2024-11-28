> Sender allowed to transmit multiple packets without waiting for ACK, but is constrained to max $N$ unacknowledged packets in pipeline

- Protocol is essentially a sliding window
![[go-back-N.png]]
- `base`: SN of oldest unacknowledged
- `nextseqnum`: Smallest unused SN

- Range `[0, base-1]`: Acknowledged packets
- Range `[base, nextseqnum-1]`: Sent packets that are not ACKed yet
- Range `[nextseqnum, base+N-1]`: Usable SNs with packets that are not sent yet
- Range `[base+N, inf)`: Unusable SNs

#### Explanation
- Limit $N$ is imposed for congestion control
- In practice, packet's SN is carried in header with fixed-length field of $k$ bits
	- Range of SNs will be `[0,2k-1]`
	- TCP has 32-bit SN field, where TCP SN count bytes in byte stream rather than packets

#### FSMs of protocol with Go-Back-N
![[gbn sender.png]]
![[gbn receiver.png]]
- GBN sender now has 3 events to respond to
	- **Call from above**
		- Sender checks if window is full
			- If not, packet is created and sent, window updated
			- If full, sender returns data to upper layer (but in real case, sender most likely would've buffered or have a sync primitive to ensure that function calls succeeds only if window is not full)
	- **Receipt of ACK**
		- ACK for SN $n$ is taken as **cumulative acknowledgement**, indicating all packets up to and including SN $n$ is acknowledged
	- **Timeout**
		- If timeout occurs, sender resends all packets previously sent but not ACKed
		- Sender above uses only 1 timer for the oldest transmitted but not ACKed packet
		- If ACK received but there are still transmitted packets that are not ACKed yet, timer resets
		- If there are no outstanding unACKed packets, timer is stopped
- GBN receiver
	- If packet $n$ received correctly and in order, receiver ACKs packet $n$ and delivers data to upper layer
	- Else, packet discarded and ACK resent for most recently received in-order packet
		- Seemingly wasteful since packet $n+1$ onwards can be buffered but ok because packet $n+1$ will be retransmitted anyways due to the **cumulative ACK**
	- Receiver just needs to maintain SN of the next in-order packet

#### Example GBN with window size=4
![[GBN with window size 4.png]]