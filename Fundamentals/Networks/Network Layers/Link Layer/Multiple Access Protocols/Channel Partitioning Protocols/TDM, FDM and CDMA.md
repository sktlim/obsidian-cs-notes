### TDM and FDM
- Time division multiplexing and frequency division multiplexing allows for partitioning of a broadcast channel's bandwidth. See [[Circuit Switching#Multiplexing in circuit-switches networks]] for more details
- TDM eliminates collisions and is completely fair, but
	- Each node is limited to average rate of $R/N$ bps even when its the only node with packets to send
	- Node must always wait for its turn even if its the only packet to be sent
- FDM avoids second disadvantage but shares the first one (that each node is limited in throughput)

### Code Division Multiple Access (CDMA)
- Assigns different codes to each node
- Each node uses code to encode the bits it sends
- CDMA has unique property of being decipherable as long as receiver knows the sender's code. See [[CSMA and CSMA-CD]]