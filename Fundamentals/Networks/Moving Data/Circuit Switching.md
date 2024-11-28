> Communication method where resources needed are reserved for the duration of the communication session between to endpoints, ensuring fixed bandwidth and consistent quality

## Operation
- Similar to telephone lines, when network establishes a circuit, it also reserves a constant transmission rate in the network's links for duration of connection
- Transmission rate is guaranteed to be constant

## Multiplexing in circuit-switches networks
![[FDM vs TDM.png]]
- Circuit in link is implemented with either **frequency-division multiplexing (FDM)** or **time-division multiplexing (TDM)**
	- **FDM**: frequency spectrum divided among connections in link using frequency bands
		- width of band is therefore bandwidth and usually has width of 4kHz
	- **TDM:** time divided into frames of fixed duration, each frame divided into fixed number of time slots

## Numerical Example of Circuit switching
### Problem
- How long does it take to send 640kbits from Host $A$ to Host $B$ ?
- **Circuit Switch Assumptions**
	- Uses TDM with 24 slots
	- bit rate = 1.536 Mbps
	- Time to establish end-to-end circuit = 500ms

### Time taken
- Each circuit has transmission rate of $1.536 / 24=64kbps$
$$
\begin{aligned}
\text{Time taken} &= \text{Time}_{\text{Transmission}} + \text{Time}_{\text{establish circuit}} \\
                  &= \frac{640}{64} + 0.5 \\
                  &= 10.5 \, \text{sec}
\end{aligned}
$$
