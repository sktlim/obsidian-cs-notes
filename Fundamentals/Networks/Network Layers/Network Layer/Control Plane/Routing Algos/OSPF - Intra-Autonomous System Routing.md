# Open Shortest Path First (OSPF)
> Dynamic, **link-state routing protocol** used within an **Autonomous System (AS)** to determine the best path for routing IP packets. It is widely adopted in large enterprise and ISP networks due to its scalability, efficiency, and support for advanced features like hierarchical design.

## Description 
- Each router constructs complete topological map of entire AS
- Each router locally runs Dijkstra to determine shortest path tree to all subnets with itself as root node
- Individual link costs configured by network admin
	- Network admin might set weights to 1 to achieve minimum-hop routing, or 
	- choose weights to be inversely proportional to link capacity to discourage traffic from using low-bandwidth links
	- In practice, network admin might use weights to achieve a specific traffic engineering outcome
- Router broadcasts routing info to all other routers in AS, and broadcasts link state info whenever there's a change in link state, as well as periodically (at least once every 30 mins)
- OSPF advertisements are contained in OSPF messages that are carried directly by IP, with an upper-layer protocol of 89 for OSPF. 
	- Thus, the OSPF protocol must itself implement functionality such as reliable message transfer and link-state broadcast. 
	- The OSPF protocol also checks that links are operational (via a HELLO message that is sent to an attached neighbor) and allows an OSPF router to obtain a neighboring router’s database of network-wide link state.

## How OSPF Works
1. **Neighbor Discovery**:
    - Routers identify their neighbors on directly connected networks using **Hello packets**.
2. **Link-State Advertisement (LSA)**:
    - Routers exchange **Link-State Advertisements (LSAs)** to share information about their directly connected links and neighbors.
3. **Link-State Database (LSDB)**:
    - Each router builds and maintains an identical **link-state database (LSDB)**, which contains the complete topology of the network.
4. **Shortest Path Calculation**:
    - Routers use **Dijkstra’s algorithm** to calculate the shortest path to each destination.
    - The results are stored in the **OSPF routing table**.
5. **Routing Updates**:
    - Updates are triggered by topology changes, not periodic broadcasts, reducing overhead.

## OSPF Hierarchy
### Areas
- OSPF divides the AS into **logical regions** called **areas** to simplify management and scale efficiently.
- An area is a group of routers that share the same topology information, reducing the amount of routing data exchanged between areas.

### Types of Areas
- **Backbone Area (Area 0)**:
    - The central hub of the OSPF hierarchy.
    - All other areas must connect to the backbone, either directly or through a virtual link.
    - Responsible for inter-area routing.
- **Regular Areas**:
    - Other non-backbone areas connected to the backbone.
    - Routers in these areas share detailed link-state information within the area but rely on the backbone for communication between areas.
- **Stub Areas**:
    - Block external routes (Type 5 LSAs) to reduce routing table size.
    - Instead, use a default route to reach external destinations.
- **Totally Stubby Areas**:
    - Block both external routes (Type 5 LSAs) and inter-area routes (Type 3 LSAs), reducing routing complexity.
- **Not-So-Stubby Areas (NSSA)**:
    - Similar to stub areas but allow limited external routing (Type 7 LSAs).
## Advantages of OSPF
- **Security**: Exchanges between OSPF routers can be authenticated
- **Multiple same-cost paths:** OSPF allows multiple paths to be used
- **Integrated support for unicast and multicast routing:** Multicast OSPF (MOSPF) uses existing OSPF link database and adds new type of link-state advertisement to existing broadcast mechanism
- **Support for hierarchy within single AS**

