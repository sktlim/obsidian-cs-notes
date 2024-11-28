> A transport layer protocol in the TCP/IP suite that provides **reliable**, **byte-stream-based**, **connection-oriented** communication using features like **three-way handshake** for connection establishment, **sequence numbers** for in-order delivery, **acknowledgments** for reliability, **flow control** to manage sender-receiver speed, and **congestion control** to prevent network overload

**Connection-oriented:** Before processes can send data, they must do a handshake first

## Notes 
- Connection here is a logical one, different from that in circuit switching. TCP common state is only in end systems. Routers see datagrams, not connections. 
- TCP provides a **full-duplex service** (Connection between A and B means that data can flow from A to B at the same time as B to A)
- TCP connection is **point-to-point**, between a single sender and single receiver. [[Multicasting]] (1 sender, multiple receivers) is not possible with TCP

## TCP Core Aspects
### Process Overview
#### TLDR
- Client and server 3 way handshakes
- **Maximum Segment Size (MSS):** Max amount of app-data in segment 
- **Maximum Transmission Unit (MTU)**: Max length of link-layer frame that can be sent by local sending host
- MTU = MSS + TCP/IP header length
#### Explanation and Example
1) Client performs 3-way handshake with server
2) Once TCP connection established, client passes a stream of data through the socket and client TCP takes over
3) TCP directs data to connection's **send buffer**
	1) TCP will grab data from send buffer and pass data to network layer (Standard is lax with when TCP does this, stating that it should "send data... at its own convenience")
	2) Max amount of <u>app-layer data</u> in a segment is **Maximum Segment Size (MSS)**, which is set by
		1) Determining length of largest link-layer frame that can be sent by local sending host (**Maximum Transmission Unit (MTU)**)
		2) Setting MSS such that TCP segment + TCP/IP header length (40 bytes) fits into single layer link frame
		3) Ethernet and PPP link-layer protocols have MTU of 1500 bytes, meaning typical value of MSS is 1460 bytes
4) TCP pairs each chunk of data with a header, making a **TCP segment**
5) Segments are passed down to network layer, where they are encapsulated in datagrams and sent into network 
6) When data arrives at receiver, recipient TCP places data in connection's **receive buffer** 
7) App reads stream of data from buffer

### TCP Segment Structure
![[TCP Segment Structure.png]]
- **Source Port (16-bits)**
- **Destination Port (16-bits)**
- **Sequence Number (32-bit)**: Used for implementing reliable data transfer
- **ACK Number (32-bit)**: Used for implementing reliable data transfer
- **Receive Window (16-bit)**: Used for **flow control**; indicates number of bytes that a field is willing to accept
- **Header-length (4-bits):** Specifies length of TCP header in 32-bit words. TCP header length variable due to Options field
- **Checksum (16-bits)**
- **Urgent Data Pointer (16-bits)**
- **Options:** Optional and variable. Used when sender and receiver negotiate MSS or window scaling factor for use in high speed networks. Time-stamping option is also defined
- **Flag (6-bits):** 
	- `ACK`: Indicate value in ACK field is valid
	- `RST`: Resets connection forcefully
	- `SYN`: Synchronizes number between sender and receiver
	- `FIN`: Gracefully terminates connection
	- `PSH`: Tells receiver to deliver data immediately rather than buffering
	- `URG`: Indicates data is urgent and should be processed immediately
- **Note:** In practice, `PSH`, `URG` and urgent data pointer not really used
	- RFC 854: Telnet utilizes the TCP URG (Urgent) flag to handle special control sequences, such as interrupt signals. When transmitting urgent commands, Telnet sets the URG flag and uses the urgent pointer to indicate the position of the urgent data within the segment. 

### TCP Sequence Numbers and Acknowledgements
![[TCP data segments.png]]
#### TLDR
- Initial Sequence Number (ISN) randomly chosen and subsequent SNs are the byte stream number of 1st byte in segment
- ACK numbers refer to the SN sender is expecting next
#### Explanation and Example
- TCP views data as unstructured, but ordered stream of bytes
- TCP randomly chooses an initial sequence number (ISN) to minimize possibility that a segment from earlier (terminated) connection is mistaken for valid segment in later connection
- Sequence numbers are over the stream of transmitted bytes (instead of segments)
- Sequence number of segment = byte-stream number of 1st byte in segment
	- TCP in sender will implicitly number each byte in data stream
	- If data stream has 500,000 bytes, MSS = 1000 bytes, **1st segment has SN 0**, **2nd has SN 1000**, and so on for the 500 packets
- ACK number host A puts in segment = SN of next byte A is expecting from B
	- **Example 1**
		- Host A received all bytes 0 - 535 from B, waiting for 536
		- When Host A sends segment to B, it puts 536 in ACK field
	- **Example 2**
		- Host A received segment containing bytes 0 - 535, and another segment containing bytes from 900 - 1000
		- Host A has not received bytes 536 - 899 and is waiting 
		- A's next segment to B will containing 536 in ACK field
	- TCP only ACKs bytes up to first missing byte in stream, TCP provides **cumulative acknowledgements** 

**Note:** In example 2, bytes are received out-of-order and RFC does not dictate what TCP should do. Usual implementation in practice is to store out-of-order bytes first and wait for missing bytes to fill gap, instead of discarding, to save on network bandwidth

![[SN and ACK number example with Telnet.png]]
- Packet 1
	- ISN = 42, IAN = 79
	- A (client) sends B(server) data
- Packet 2
	- B sends expected packet 79 to A
	- B sends ACK 43: 0-42 ACKed, expecting 43
	- B (server) sends A (client) data
		- ACK is **piggybacked** onto the server-to-client data segment
- Packet 3
	- A sends expected packet 43 to B
	- A sends ACK 80: 0-79 ACKed, expecting 80

### Round-Trip Time (RTT) Estimations and Timeout
- TCP uses timeout/retransmit mechanism to recover from lost segments
- What should be my length of timeout intervals?
#### Initial Idea
- `SampleRTT`: Amount of time between when segment is sent and when ACK for segment is received
- Most TCP implementations take only one `SampleRTT` measurement at a time
- `SampleRTT` value fluctuates due to congestion
- TCP maintains an average (**exponential weighted moving average (EWMA)**) of `SampleRTT`, `EstimatedRTT` $$EstimatedRTT=(1−α)⋅EstimatedRTT+α⋅SampleRTT$$
- where recommended $α = 0.125$
- TCP also measures variability of `SampleRTT`, using `DevRTT` (EWMA of difference between `SampleRTT` and `EstimatedRTT`) $$DevRTT=(1−β)⋅DevRTT+β⋅|SampleRTT−EstimatedRTT|$$
- where recommended $β = 0.25$
- Therefore, $$TimeoutInterval=EstimatedRTT+4⋅DevRTT$$
	- Initial value of $TimeoutInterval = 1 sec$ is recommended
	- When timeout occurs, $TimeoutInterval$ is doubled to prevent premature timeout for a subsequent segment soon to be ACKed

### Reliable Data Transfer
- TCP creates reliable data transfer on top of IP's unreliable best-effort service
- Ensures that data is 
	- uncorrupted
	- without gaps
	- without duplication
	- in sequence
- Recommended TCP timer management procedures use only a single retransmission timer

```c
/* Assume sender is not constrained by TCP flow or congestion control, that data from above is less
   than MSS in size, and that data transfer is in one direction only. */

NextSeqNum = InitialSeqNumber
SendBase = InitialSeqNumber

loop (forever) {
    switch(event)
    
    event: data received from application above
        create TCP segment with sequence number NextSeqNum
        if (timer currently not running)
            start timer
        pass segment to IP
        NextSeqNum = NextSeqNum + length(data)
        break;

    event: timer timeout
        retransmit not-yet-acknowledged segment with smallest sequence number
        start timer
        break;

    event: ACK received, with ACK field value of y
        if (y > SendBase) {
            SendBase = y
            if (there are currently any not-yet-acknowledged segments)
                start timer
        } else {
            /* A duplicate ACK for already ACKed segment */
            increment number of duplicate ACKs received for y
            if (number of duplicate ACKs received for y == 3) {
                /* TCP fast retransmit */
                resend segment with sequence number y
            }
        }
        break;
}

```
- Things TCP does differently
	- **Doubling timeout interval**
		- If timer timeouts, TCP will retransmit and double the expiration time 
		- Provides limited congestion control as timer expiration most likely caused by congestion in network
	- **Fast retransmit**
		- Timeout triggered retransmissions can be relatively long
		- Fortunately, sender can detect packet loss before timeout event by noting duplicate ACKs
			- When TCP receiver receives segment with SN larger than next expected SN, it detects gap in stream
			- Gap could be due to lost or reordered packets in network
			- Receiver simply re-ACKs the last **in-order** byte it received
		- As sender often send many segments back to back, if 1 segment lost there will be many back-to-back duplicate ACKs 
			- If sender receives 3 duplicate ACKs for same data, it takes it that segment with SN after SN of data ACKed 3 times is lost 
		- If 3 duplicate ACKs received, sender does a **fast retransmit**, retransmitting the missing segment before segment timer expires
		- See RFC 5681 for ACK generation recommendations
	- **Go-Back-N or Selective Repeat?**
		- TCP uses cumulative ACKs like GBN
		- However, many TCP implementations will buffer out-of-order segments
		- If some segments were sent and received and one of the ACKs for segment $n$ were lost, GBN will resend everything from segment $n$ onwards, while TCP will only retransmit segment $n$. TCP would not even retransmit segment $n$ if the ACK for segment $n+1$ arrived before timeout for $n$ 
		- Proposed modification for TCP **Selective Acknowledgement** allows TCP receiver to ACK out-of-order segments selectively instead of cumulatively. When this is combined with selective retransmission (skipping selectively ACKed segments) TCP looks like SR protocol
		- **TCP recovery mechanism best characterized as hybrid of GBN and SR**

### TCP Flow Control
- Hosts on each side set aside a receive buffer for TCP connection 
- Buffer might overflow if rate of data being sent > rate of app reading data
- TCP provides **flow-control service** to eliminate the possibility of buffer overflow
	- Done by having sender maintain variable **receive window** `rwnd`
	- Used to give sender an idea of how much free buffer space there is at receiver

- Example
	- Host A sends data to Host B
	- Host B has a receive buffer `RcvBuffer`
	- `LastByteRead`: number of last byte in data stream read from buffer by app in B
	- `LastByteRcvd`: number of last byte that arrived from network and that has been placed in receive buffer in B
	- To prevent overflow, $LastByteRcvd - LastByteRead \leq RcvBuffer$
	- `rwnd` is set to spare room in buffer$$rwnd = RcvBuffer - (LastByteRcvd - LastByteRead)$$
	- `rwnd` is dynamic as spare room changes
	- **B tells A the amount of spare room it has by sending `rwnd` in the receive window field of every segment it sends to A**
	- Host A in turn keep track of `LastByteSent` and `LastByteAcked`. Difference between these 2 variables is the amount of unACKed data A has in the connection
	- As long as this amount is lesser than `rwnd`, we know that receive buffer for B is not overflowed
	- Essentially, host A makes sure that $$LastByteSent - LastByteAcked \leq rwnd$$
- Minor Technical problem with above scheme
	- If B's buffer is full and `rwnd = 0`
	- B tells A that its full but B has nothing to send A
	- As B empties buffer, TCP does not inform A that buffer is emptying, meaning now A is blocked and can't send data 
	- **Solution:** TCP spec requires A to send segments with 1 data byte when B's receive window is zero
		- Segments will be ACKed by receiver 
		- Once buffer empties out, ACKs will contain nonzero `rwnd` value

### TCP Handshake and Connection Management
#### Establishing Connection
1) Client-side TCP sends special TCP segment (**SYN segment**) over to server-side TCP
	1) Segment contains no app-data, and just has SYN flag set to 1
	2) Client-side TCP randomly chooses an Initial Sequence Number (ISN) `client_isn`
	3) Segment is encapsulated within IP datagram and sent to server
2) Server-side TCP receives IP datagram and extracts TCP SYN segment
	1) Server-side TCP creates its own TCP connection variables and buffers
	2) Server-side TCP creates connection-granted segment (**SYNACK segment**) with no app data, SYN flag set to 1, its own random ISN `server_isn` , and ACK field set to `client_isn+1`
3) Client-side TCP reads the SYNACK segment
	1) Client-side TCP creates its own TCP connection variables and buffers
	2) Client side sends segment to ACK SYNACK, with 
		1) SYN = 0, (since connection established)
		2) SN = `client_isn+1`, 
		3) ACK = `server_isn+1`
		4) App-data 

#### Closing Connection
1) Client chooses to close connection and sends special TCP shutdown segment (**FIN Segment**) to server
	1) FIN Segment has FIN flag set
2) Server receives this segment and sends client an ACK in return
3) Server then sends its own shutdown FIN segment 
4) Client receives and ACK server's FIN segment
5) All resources deallocated and connection closed

![[TCP states.png]]
- **FIN_WAIT_1 State:** Client waits for ACK from server for FIN segment
- **FIN_WAIT_2 State:** Client waits for FIN from server
- **TIME_WAIT**: Lets TCP client resend final ACK in case packet lost (typical values are 30s, 1min, 2mins). After the wait, connection closes and resources released

![[Server-side TCP states.png]]

- Above cases assume that both client and server are prepared to communicate i.e. server listening on port which client sends its SYN segment to
- When host receives TCP segment whose port numbers/ source IP doesn't match ongoing sockets in host
	- Host will send special reset segment (RST) to source (has RST flag set to 1)
	- Essentially tells source to not resend that packet because it does not have a socket available for it
- **Note:** If UDP host receives UDP packet whose destination port number doesn't match, host sends a special ICMP datagram

