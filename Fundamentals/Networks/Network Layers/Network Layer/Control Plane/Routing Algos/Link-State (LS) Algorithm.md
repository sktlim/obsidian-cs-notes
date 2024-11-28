> **Centralized** routing algorithm

In practice, network topology and all links are known through the **link-state broadcast algorithm** (having each node broadcast link-state packets to all other nodes in the network), with each link-state packet containing identity and cost of all its attached links 

### Examples of LS algorithms
- Dijkstra's algorithm (see textbook for network specific example)
- Prim's algorithm

## Route Oscillation from LS algorithms
> Fluctuation of routes due to inconsistent routing decisions or updates, often caused by conflicting route policies or topology changes
### Example
![[LS oscillation example state 1.png|200]]
- node $z$ and $x$ originates unit of traffic meant for $w$
- node $y$ injects traffic $e$ to $w$ 

![[LS oscillation example state 2.png|200]]
- node $y$ determines clockwise path has cost $1$, while counter-clockwise path has (as of figure a) cost of $1+e$, so its least cost path is now clockwise
- node $x$ determines clockwise path has cost 1, while counter-clockwise-path has cost of 1+e, so it also switches
- Next run will be clockwise

![[LS oscillation example state 3.png|200]]
- Now the opposite occurs and they'll detect that counter-clockwise path is better, and oscillate between these 2 routes

### Solution
- Mandate that link costs not be determined based on amount of traffic carried
	- Unacceptable because one goal of routing is to avoid congested routes
- Ensure not all routers run LS algorithm at same time
	- More reasonable, and can be accomplished by routers randomizing the time they send out link advertisement