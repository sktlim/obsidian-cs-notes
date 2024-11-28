![[Router Architecture.png]]
- **Input ports**: 
	- Performs physical layer function of terminating incoming physical link at router
	- Performs link-layer functions to interoperate with link layer at other side of link
	- **Important:** Performs lookup function and consult forwarding table to determine which output port to route the arriving packet to via switch fabric
- **Switching fabric:**
	- Connects router's input ports to output ports
- **Output ports:**
	- Stores packets received from switch fabric and transmit packets on outgoing link
- **Routing processor:**
	- Executes routing protocols
	- Maintains routing tables and attached link state info
	- Computes forwarding table for router
	- **For SDN routers:** routing processor responsible for communicating with remote controller
	- The forwarding table is copied from the routing processor to the line cards over a separate bus (e.g., a PCI bus) indicated by the dashed line from the routing processor to the input line cards


