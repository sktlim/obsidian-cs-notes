- Each side of TCP connection has send and receive buffers, as well as a bunch of variables: `LastByteReceived`, `rwnd`, and so on
- TCP Control Mechanism at sender has variable control window `cwnd`
	- Imposes constraint on rate at which TCP sender can send traffic into the network
- Amount of unacknowledged data at sender may not exceed minimum of `rwnd` and `cwnd`
$$LastByteSent - LastByteAcked \leq min(cwnd,rwnd)$$

- To focus on congestion control instead of flow control, we assume that 
	- TCP receive buffer is large to the point where the receive window constraint can be ignored so that `cwnd` is the only factor we have to consider
	- Sender always has data to send i.e. all segments in congestion window are sent

## Intuition
- TCP uses `cwnd` to control sending rate as `cwnd` controls the amount of unacknowledged data at sender
- **Lost segment implies congestion, so TCP sender's rate should be decreased on segment loss**
	- **Loss event:** Timeout or 3 duplicate ACKs 
- **Self-clocking: ACKed segment indicates network is delivering segment to receiver, so sender's rate can be increased**
- **Bandwidth probing:** 
	- TCP's strategy is to increase `cwnd` until loss events occur, at which point transmission rate is decreased, and then begin to probe again to see if congestion onset rate has changed

## TCP Congestion Algorithm
![[TCP Congestion Control FSM.png]]
### Slow Start
1) When TCP started, `cwnd` initialized to 1 MSS, resulting in sending rate of MSS/RTT
2) Increases by 1 MSS every time transmitted segment is ACKed (Exponential growth in send rate)
	1) `cwnd` doubles approximately every RTT
3) Growth ends when
	1) Loss event indicated by timeout: 
		1) Sets slow start threshold `ssthresh` to `cwnd/2` 
		2) `cwnd` set to 1 and begin process anew
	2) Loss event indicated by 3 duplicate ACKs:
		1) TCP performs fast retransmit and enters fast recovery mode
	3) `cwnd` == `sshthresh`: 
		1) TCP transitions into congestion avoidance mode
![[TCP Slow start.png|300]]
### Congestion Avoidance
1) TCP increases `cwnd` by 1 MSS every RTT
	1) Common approach for sender to increase `cwnd` by MSS bytes whenever new ACK arrives
2) Linear increase ends when timeout occurs
	1) `ssthresh` updated to `cwnd/2`
	2) `cwnd` set to 1 MSS
	3) If 3 dup ACKs occur, 
		1) `ssthresh` = `cwnd`/2
		2) `cwnd` = `ssthresh` + 3 * MSS
		3) Transit into fast recovery

### Fast Recovery
1) `cwnd` increased by 1 MSS for every duplicate ACK received for missing segment that caused fast recovery transition
2) If ACK arrives for missing segment,
	1) TCP enters congestion-avoidance state 
3) If timeout event occurs, 
	1) TCP enters slow start timeout after performing same action for `ssthresh` and `cwnd`

### Conclusion
- Fast recovery is recommended, but not required component of TCP
- TCP congestion control referred to as an **additive-increase, multiplicate decrease (AIMD)** form of congestion control

## Comparison of various TCP congestion control algorithms

| **Algorithm**    | **Key Features**                                                                                                                 | **Advantages**                                                                                         | **Disadvantages**                                                                                                 |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| **TCP Tahoe**    | - Uses **slow start**, **congestion avoidance**, and **fast retransmit**.- Drops congestion window (cwnd) to 1 on packet loss.   | - Simple and robust.- Introduced foundational concepts for congestion control.                         | - Inefficient recovery from packet loss as it resets cwnd completely.- Poor performance in high-latency networks. |
| **TCP Reno**     | - Improves Tahoe by adding **fast recovery**.- Retransmits lost packets without dropping cwnd to 1.                              | - Faster recovery from single packet loss.- Better performance in lightly congested networks.          | - Struggles with multiple packet losses in the same window.- Degrades performance in high loss scenarios.         |
| **TCP New Reno** | - Enhances Reno by introducing **partial ACKs** during fast recovery.- Handles multiple packet losses more efficiently.          | - Better performance in high-loss environments.- More efficient congestion window adjustment.          | - Still has limitations with detecting losses and handling delayed ACKs.                                          |
| **TCP Vegas**    | Focuses on **delay-based congestion control** rather than loss-based.- Detects congestion earlier by monitoring RTT changes.     | - Higher throughput in low-loss networks.- Better resource utilization compared to loss-based methods. | - Performance degrades in networks with competing loss-based TCP flows.- Sensitive to network jitter.             |
| **FAST TCP**     | A high-speed, delay-based algorithm for high-bandwidth, long-latency networks.- Relies on stable RTT estimation for adjustments. | - Excellent performance in high-speed, long-delay networks.- Smooth congestion control.                | - Less effective in networks with competing loss-based flows.- Requires more sophisticated tuning.                |
| **TCP Hybla**    | Designed for high-latency satellite networks. Adjusts congestion window growth to match performance of low-latency networks.     | - Improves performance in satellite and long-delay networks.- Mitigates unfairness to long RTT flows.  | - Can lead to aggressive behavior in mixed network environments.                                                  |

|**Feature**|**Reno**|**NewReno**|**CUBIC**|**BBR**|**Vegas**|
|---|---|---|---|---|---|
|**Year Introduced**|1990|1999|2006|2016|1994|
|**Congestion Control Type**|Loss-based|Loss-based|Loss-based|Rate-based|Delay-based|
|**Initial Concept**|AIMD (Additive Increase Multiplicative Decrease)|AIMD with improved retransmission|Optimized for high-bandwidth, long-distance networks|Based on bandwidth estimation|RTT (Round-Trip Time) estimation|
|**Congestion Window (cwnd)**|Linear increase, halve on loss|Linear increase, halve on loss, fast recovery improvements|Increases more aggressively in high BDP paths|Based on maximum bottleneck bandwidth and RTT|Adjusts using RTT trends|
|**Response to Packet Loss**|Halve cwnd|Halve cwnd and avoid retransmission ambiguities|Halve cwnd but allows for high throughput post-recovery|Adjusts bandwidth estimate|Maintains cwnd if delay remains low|
|**Advantages**|Simple, well-tested|Handles multiple losses better|High throughput, scalable|Efficient bandwidth utilization, low latency|Efficient in low-loss environments|
|**Disadvantages**|Inefficient in high-loss networks|Similar inefficiency as Reno but better handling of multiple losses|May overestimate capacity in certain conditions|Requires accurate RTT and bandwidth estimation|Can underperform in lossy networks|
|**Best Use Cases**|General-purpose, low- to mid-bandwidth|Low- to mid-bandwidth with potential losses|High-bandwidth, high-latency networks|Latency-sensitive, real-time applications|Stable, low-latency, low-loss environments|

### Key Points:

1. **Tahoe** is the simplest and foundational algorithm, while **Reno** introduced fast recovery for better efficiency.
2. **New Reno** refines Reno's performance in handling multiple packet losses.
3. **Vegas** shifts to delay-based control for earlier congestion detection but struggles in mixed traffic.
4. **FAST TCP** and **Hybla** are designed for specialized environments like high-speed or satellite networks.

### Selection Guide:

- **Loss-based algorithms** (Tahoe, Reno, New Reno): Suitable for general-purpose networks where packet loss is the primary congestion signal.
- **Delay-based algorithms** (Vegas, FAST): Preferred for low-loss, high-speed networks or where delay is a better congestion indicator.
- **Specialized algorithms** (Hybla): Designed for environments like satellite networks where latency is inherently high.

## TCP Throughput
![[AIMD Congestion Control.png]]
- To analyze average TCP throughput given saw-toothed behavior
	- `cwnd` = $w$ bytes
	- Current round-trip time = $RTT$ seconds
	- Transmission rate = $w/RTT$ bytes/sec
	- TCP probes for additional bandwidth until loss event occurs at $w=W$ bytes
	- $\therefore transmission \space rate_{TCP} = \left[ \frac{W}{2 \cdot RTT}, \frac{W}{RTT} \right]$
	- Average throughput = $0.75\frac{W}{RTT}$

### TCP over high bandwidth paths
- Given TCP connection with 1500 byte-segments, 100ms RTT, we want to send data at 10Gbps
- Average `cwnd` size needs to be 83,333 segments
- Packet drop rate would need to be 1 of 5,000,000,000 segments for data to hit 10Gbps, so TCP for high speed bandwidths is still a topic that is being researched

## Fairness
> Each connection gets an equal share of bandwidth, or 
> Given $K$ TCP connections, all passing through bottleneck link with transmission rate $R$ bps, congestion control mechanism is **fair** if average transmission rate of each connection is $R/K$ bps

### TCP Fairness
TCP AIMD congestion control converges to being fair under idealized circumstances
- **Proof**
	- ![[TCP fairness.png]]
	- Given 2 connections sending data through a bottleneck link, with no other TCP/UDP datagrams or other connections beside the 2, and these 2 connections having the same MSS and RTT, ignore slow-start phase and assume connections are operating in congestion avoidance (CA AIMD) mode at all times
	- At given point of time, throughput is at point A
		- Below full bandwidth utilization, so both 1 and 2 will increase their congestion window size
		- Joint throughput progresses along 45 degree line (1 MSS per RTT)
	- Throughput reaches point B and loss event occurs
		- Loss event causes 1 and 2 to half their `cwnd` sizes, causing a multiplicative decrease
		- Combined throughput drops to point C
	- Throughput starts the process again, and combined throughput will slowly fluctuate until it reaches equal bandwidth share line 

- Conditions do not hold in practice, and different connections can have very unequal portions of bandwidth
- Sessions with smaller RTT are able to grab available bandwidth more quickly as they increase their `cwnd` size faster and thus enjoy higher throughput
- UDP sessions also have no concept of congestion control and just blasts away and occasionally lose packets
- Parallel TCP connections can also get higher bandwidth

## Explicit Congestion Notification (ECN)
![[Explicit Congestion Notification.png]]
- At network layer, 2 bits in Type of Service (TOS) Field in IP datagram header used for ECN 
- ECN bit carried in IP datagram to receiver, which then informs sender by setting the ECE (ECN Echo) bit in a ACK segment
- Sender halves congestion window, and sets CWR (congestion window reduced) bit in next segment