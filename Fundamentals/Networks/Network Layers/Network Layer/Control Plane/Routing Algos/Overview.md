Routing problems are basically graph problems
- Nodes represent routers
- Edges represent physical link between routers
- Cost represents length of link, link speed, or monetary cost associated with link

> **Autonomous System (AS):** Routers organized into groups that are under same administrative control

- Identified by globally unique **AS Number (ASN)** assigned by ICANN
- Routing algorithms inside AS known as **intra-autonomous routing protocol**

Types of routing algorithms
- **Centralized routing algorithm**
	- Computes least-cost path between source and destination using **complete, global knowledge** of network
	- Algorithms with global state info are referred to as **link-state (LS) algos**
- **De-centralized routing algorithm**
	- Calculation of least-cost path carried out in iterative, distributed manner by routers
	- No node has complete information about costs of all networks
- **Static routing algorithm**
	- Routes change very slowly over time, usually due to human intervention
- **Dynamic routing algorithm**
	- Changes paths as network traffic/topology changes
	- Can change periodically or in direct response to topology/ link changes
	- More responsive to network change, but more susceptible to routing loops and route oscillation
- **Load-sensitive algorithm**
	- Link costs vary dynamically to represent congestion
- **Load-insensitive algorithm**
	- Many of today's routing algorithms are load insensitive 