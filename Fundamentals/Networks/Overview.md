### Introduction
- End systems (aka hosts) are connected by network of **communication links** and **packet switches**
	- Packets are packages of info sent by source to destination (analogous to trucks)
	- Links include different types of physical media e.g. fiber, cable, copper wire (analogous to highways)
	- Packet switches include routers and link-layers switches, which forward packets to their destinations (analogous to intersections)
	- End systems are analogous to buildings
		- End systems provide a socket interface that specifies how program running on one end system asks internet infrastructure to deliver data to specific destination program running on another end system
		- Set of rules that sending program must follow so that internet can deliver data to destination program

### Network Resources
- Network resources are <u>statistically multiplexed</u>
- **Statistical multiplexing:** Combining demands to share resources efficiently
	- Means you don't provision resources for worst case; share resources and pray worst case doesn't happen
	- Based on: peak of aggregate demand << aggregate of peak demands

#### Sharing network resources
1) Reservations 
	1) End hosts explicitly reserve bandwidth when needed
	2) Resources shared between **flows** currently in system
	3) **Circuit switching**
		1) `source` sends reservation request to `dest`
		2) switches establish a path
		3) `source` starts sending data
		4) `source` sends a "teardown path" message
	4) Pros
		1) Better app performance 
		2) More predictable and understandable behavior
2) Best-effort
	1) Send data packets and pray for the best
	2) Resources shared between **packets** currently in system
	3) **Packet switching**
		1) Allocate resources to each packet independently
	4) Pros
		1) More efficient use of network capacity, subject to burstiness of traffic sources

### Network of Networks
![[Network Structure 5 - Internet Diagram.png]]
- To create a model of the Internet
- **Naive approach**
	- Each access ISP connects with every other access ISP
	- Too costly due to too many communication links required
- **Network Structure 1**
	- All access ISPs connects to a **single global transit ISP**
	- Costly to create global ISP
	- Leads to competition from other firms to build this global ISP
- **Network Structure 2**
	- Many access ISPs connecting to **multiple** global transit ISPs
	- Global transit ISPs must themselves interconnect
	- 2-tier hierarchy with global transit providers at the top tier (**Tier 1 ISP**) and access ISPs at bottom tier
	- More realistic to have regional ISPs as no ISP has presence everywhere
		- regional ISPs then connect Tier 1 ISP to access ISP
- **Network Structure 3**
	- Multi-tiered hierarchy that accounts for regional ISPs
- **Network Structure 4**
	- Points of Presence (PoPs), multi-homing, peering, and Internet Exchange Points (IXPs) must be added to resemble internet
		- PoPs
			- Group of routers in provider's network where customer ISPs can connect to provider ISP
			- exists in all levels, except in access ISPs
		- Multi-home
			- Any ISP (except Tier 1) may choose to connect to 2 or more ISPs
		- Peer
			- Pair of ISPs at same level can connect their networks together (peer) so that traffic between them is via direct connection instead of upstream intermediaries
			- Typically settlement free (no payment)
		- IXPs
			- Meeting point where multiple ISPs can peer
			- Stand-alone building with its own switches
- **Network Structure 5 (Describes Internet)**
	- Adds content-provider networks like Google

## Network Map
- 