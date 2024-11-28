> **Border Gateway Protocol (BGP):** primary routing protocol used on the **Internet** for exchanging routing information between autonomous systems (AS). It is a **path-vector protocol** that focuses on ensuring scalability, reliability, and policy-based routing for large and complex networks.

## Description
- In BGP, packets not routed to specific destination address, but instead to CIDRized prefixes (representing subnets/ collection of subnets)
- A router's forwarding table will contain entries in the form of $(x,l)$ where $x$ is the prefix and $l$ is an interface number for one of the router's interface
- BGP allows routers to:
	1) **Obtain prefix reachability information from neighboring AS:**
		1) Allows each subnet to advertise its existence to other subnets
	2) **Determine "best" route to prefixes**
		1) Router will locally run a BGP route-selection procedure to determine best route based on policy and reachability information

## Advertising BGP Route Info
![[BGP advert example.png]]
- For each AS, each router is either a **gateway router** or **internal router**
	- **Gateway Routers:** 1c, 2a, 2c, 3a
	- **Internal Routers:** 1a, 1b, 1d, ...

- To advertise subnet x, 
	- AS3 sends BGP message to AS2, saying that x exists and is in AS3
	- AS2 sends BGP message to AS1, saying that x exists and is in AS3 and you can get there through AS2

![[eBGP and iBGP connectinos.png]]
- The ASes don't do this themselves, but the routers do
	- Pairs of routers exchange information over semi-permanent TCP over port 179
	- Each connection is known as a **BGP connection**
		- Connection that span 2 ASes is known as **external BGP (eBGP)** connection
		- Connection within an AS is known as **internal BGP (iBGP)** connection
- **Note:** iBGP connections don't always correspond with physical links

## Choosing best path
- **route:** prefix + attributes
- When router advertises prefix across BGP connection, it includes **BGP attributes**
	- **AS-PATH:** contains list of AS that advertisement passed
		- When prefix passed to AS, AS adds its ASN to existing list in AS-path
		- Uses AS-PATH to detect and avoid looping advertisements; if own AS in path list, reject advertisement
	- **NEXT-HOP:** IP address of router interface that begins AS path
		- ![[NEXT-HOP.png|500]]
		- Routes from AS1 to AS3
			- Through AS2: NEXT-HOP is router 2a
			- Directly to AS1: NEXT-HOP is router 3d
	- **Local Preference**:
	    - Used within an AS to prioritize routes. Higher values are preferred.
	- **Multi-Exit Discriminator (MED)**:
	    - Suggests the preferred path into an AS when multiple entry points are available. Lower values are preferred.
	- **Origin**:
	    - Indicates how the route was learned (e.g., IGP, EGP, or incomplete).

## BGP Routing Algorithms
### Hot Potato Routing
![[Hot potato routing.png]]
1) Route chosen is route with least-cost to the NEXT-HOP router beginning route
2) When traffic enters an AS, the router forwards the packet to the **nearest egress point** (the closest connection to a neighboring AS), regardless of whether that egress point is optimal for the packet's final destination.

### Route Selection Algorithm
