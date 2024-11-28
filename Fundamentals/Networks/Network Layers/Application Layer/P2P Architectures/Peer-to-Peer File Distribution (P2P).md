> Minimal reliance on always-on server infrastructure, instead relying on intermittently connected hosts to communicate directly. Most popular P2P protocol is BitTorrent

## Scalability 
P2P is by nature self-scaling

Let
- Upload rate of server access link be $u_{s}$
- Upload rate of ith peer's access link be $u_{i}$
- Download rate of ith peer's access link be $d_{i}$
- Size of file to be distributed be $F$
- Number of peer be $N$
- **Distribution Time $D_{cs}$:** Time taken to get a copy to $N$ peers

Assume
- Internet core has infinite bandwidth (All bottlenecks in access network)
- Servers and clients not participants in other network apps

### Client-server architecture
1) Server must transmit file to all $N$ peers $\rightarrow$ Total number of bits server needs to transfer = $NF$ 
2) Time to distribute file = $NF/u_{s}$
3) Let $d_{min}$ be peer with slowest download speed
4) Minimum $D_{cs} = F/d_{min}$
$$\therefore D_{cs} \geq max\left( \frac{NF}{u_{s}}, \frac{F}{d_{min}} \right)$$
- Distribution time increases linearly with $N$

### P2P architecture
- Server must send each bit of file at least once into access link 
	- Minimum $D_{P2P} = F/u_{s}$
- Peer with lowest download rate takes at least $F/d_{min}$ time to get all bits
	- Minimum $D_{P2P} = F/d_{min}$
- Total upload capacity $u_{total} = u_{s} +u_{1} + \dots +u_{N}$
	- Minimum $D_{P2P} = NF/u_{total}$
$$\therefore D_{P2P}\geq max\left( \frac{F}{u_{s}}, \frac{F}{d_{min}}, \frac{NF}{u_{total}} \right)$$
- If each peer redistributed each bit as soon as it got it, scheme that achieves lower bound exists
- In reality, where chunks are redistributed instead of individual bits, above equation is good approximation of actual minimum distribution time

In general, P2P offers lower distribution time as compared to client-server architecture


