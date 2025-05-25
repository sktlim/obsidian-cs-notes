>**Point-to-point Link:** Single sender, single receiver
>**Broadcast Link:** multiple sending and receiving nodes connected to same single broadcast channel

## Multiple Access Protocol
- If more than 2 nodes transmit at once to a broadcast channel, transmitted frames collide at receivers and none of receiving nodes can make sense of what was transmitted

- A multiple access protocol should have the following characteristics:
	- When only one node has data to send, it sends it at a throughput of $R$ bps
	- If $M$ nodes have data to send, average throughput should be $R/M$ bps; However, this does not mean that each node always has instantaneous throughput of $R/m$ bps
	- Protocol decentralized (no master node)
	- Protocol simple (inexpensive to implement)

## Channel Partitioning Protocols


## Random Access Protocols
- Transmitting node always transfers at full rate of channel
- When there is a collision, nodes that collided retransmit the frame after a random delay



### Taking-Turns Protocol
- Polling protocol
	- Master node polls each node in round-robin manner, telling them they have x time to transmit
- Token-passing protocol
	- Passing around a token to signify that they can transmit