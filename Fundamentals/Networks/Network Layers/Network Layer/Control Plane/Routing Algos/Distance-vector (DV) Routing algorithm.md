> **Decentralized** algorithm

- Distributed
	- Each node receives some info from its directly attached neighbors, performs calculation, and distributes result of calculation back to neighbors
- Iterative
	- Algorithm continues until no information is exchanged anymore between neighbors
- Asynchronous
	- Does not require nodes to operate in sync with each other

## Process
### Key Concepts
1. **Bellman-Ford Equation**:
   - The cost of the least-cost path from node $x$ to $y$, $D_x(y)$, is given by:
     $$
     D_x(y) = \min_v \{ c(x, v) + D_v(y) \} \tag{5.1}
     $$
   - The **minimum** is taken over all neighbors $v$ of node $x$.
   - **Intuition**: The least-cost path to $y$ is the minimum of the cost to reach any neighbor $v$, plus the least-cost path from $v$ to $y$.

2. **Practical Importance**:
   - The solution to the Bellman-Ford equation provides entries for the **forwarding table** of node $x$:
     - If $v^*$ is the neighbor achieving the minimum in Equation (5.1), $v^*$ becomes the **next hop** for destination $y$.

3. **Distance Vector**:
   - Node $x$ maintains:
     - **Cost to each directly attached neighbor $v$:**
	       ${c(x, v)}$
     - **Distance vector $D_x$:**
	       ${D_x = [D_x(y) : y \in N]}$
       Contains $x$'s estimates of the cost to all destinations $y$ in the network $N$.
     - **Neighbors' distance vectors $D_v$:**
       ${D_v = [D_v(y) : y \in N] \quad \text{for each neighbor } v \text{ of } x}$

4. **Update Step**:
   - When node $x$ receives a new distance vector from a neighbor $w$, it updates its distance vector as: $$
     D_x(y) = \min_v \{ c(x, v) + D_v(y) \}, \quad \forall y \in N
     $$
   - If $D_x(y)$ changes, node $x$ sends its updated distance vector to its neighbors.

---

### Verification Example
![[example graph.png]]
1. **Graph Setup**:
   - Source node $u$, neighbors $v, x, w$.
   - Costs:
     ${c(u, v) = 2, \quad c(u, x) = 1, \quad c(u, w) = 5}$
   - Distance vector values:
     ${D_v(z) = 5, \quad D_x(z) = 3, \quad D_w(z) = 3}$

2. **Bellman-Ford Calculation**:
   - Compute $D_u(z)$:
     $$
     D_u(z) = \min \{ c(u, v) + D_v(z), \, c(u, w) + D_w(z), \, c(u, x) + D_x(z) \}
     $$
   - Substitute values:
     $$
     D_u(z) = \min \{ 2 + 5, \, 5 + 3, \, 1 + 3 \} = \min \{ 7, 8, 4 \} = 4
     $$
---
### Another Example from Textbook
![[DV algorithm example.png]]

---

### Algorithm Mechanics

1. **Initialization**:
   - Each node $x$ starts with $D_x(y)$ initialized to a large value (infinity) for all $y$, except $D_x(x) = 0$.

2. **Neighbor-to-Neighbor Communication**:
   - Nodes send their distance vectors to directly connected neighbors periodically.
   - Updates occur asynchronously as distance vectors are received.

3. **Convergence**:
   - Updates propagate through the network, ensuring that $D_x(y)$ converges to the true least-cost path for all $x$ and $y$.

---

### Summary

- The Bellman-Ford equation:
  $$
  D_x(y) = \min_v \{ c(x, v) + D_v(y) \}
  $$
  provides the foundation for distance vector (DV) routing.
- Each node maintains and updates its **distance vector** and forwards it to neighbors for iterative updates.
- The forwarding table is derived from the neighbor $v^*$ that minimizes the cost in the Bellman-Ford equation.

## Algorithm
```plaintext
Initialization:
for all destinations y in N:
    D_x(y) = c(x, y)                 /* if y is not a neighbor then c(x, y) = ∞ */
for each neighbor w:
    D_w(y) = ? for all destinations y in N
for each neighbor w:
    send distance vector D_x = [D_x(y): y in N] to w

loop
    wait (until I see a link cost change to some neighbor w or
          until I receive a distance vector from some neighbor w)

    for each y in N:
        D_x(y) = min {c(x, v) + D_v(y)}

    if D_x(y) changed for any destination y:
        send distance vector D_x = [D_x(y): y in N] to all neighbors

forever
```

## Link-cost changes and Link Failure
### Count-to-infinity problem
- When node running DV algorithm detects change in link cost from itself to neighbor, it updates its distance vector, and if there's a change in least cost path, informs its neighbors of new distance vector
- Introduces possibility of **count-to-infinity problem**
	- Routing loops cause routers to continuously increase cost to destination, potentially forever
	- Happens due to delayed propagation of failure information in network

### How It Happens
1. **Initial State**:
    - Router $A$ knows a path to a destination $D$ with cost $c$, and this information is shared with neighbor $B$.
    - Router $B$ uses $A$ as its next hop to reach $D$.
2. **Failure**:
    - The link between $A$ and $D$ fails.
    - $A$ updates its cost to $D$ as infinity (∞) and removes $D$ from its routing table.
3. **Incorrect Update**:
    - Before $B$ receives the failure information from $A$, it advertises its old route to $D$ (via $A$) with cost $c$.
    - $A$, believing $B$'s advertisement, updates its route to $D$ with a cost of $c+1$, thinking $B$ has a valid path.
4. **Infinite Loop**:
    - This process repeats as $B$ and $A$ continually increment the cost to $D$ in a loop.

### Poisoned Reverse
> Advertises unreachable routes with a cost to infinity to prevent loops. 
> 
> **Only works for loops involving 2 nodes**

1. **Initial Setup**:
   - Router $A$ has a direct route to destination $D$ with cost 1.
   - Router $B$ learns about $D$ from $A$ and updates its route to $D$ via $A$ with cost 2.

2. **Link Failure**:
   - The link between $A$ and $D$ fails.
   - $A$ marks the route to $D$ as unreachable (cost $\infty$).

3. **Poisoned Reverse**:
   - $A$ advertises the route to $D$ back to $B$ with a cost of $\infty$.
   - $B$, upon receiving this update, does not consider $A$ as a valid next hop to $D$.

## DS vs LV algorithms
| **Attribute**            | **Link-State (LS) Routing**                                                                                           | **Distance Vector (DV) Routing**                                                                                         |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Communication**        | Requires **global information**: Each node broadcasts the costs of its directly connected links to all other nodes.   | Requires **local information**: Each node communicates only with directly connected neighbors, sharing least-cost paths. |
| **Message Complexity**   | O(\|N\|\|E\|) messages are sent for each node to know cost of each link                                               | Number of neighbors                                                                                                      |
| **Response to Changes**  | New link costs are broadcast to all nodes.                                                                            | Updates propagate only to neighbors, and only if they result in changes to least-cost paths.                             |
| **Speed of Convergence** | O($N^2$)                                                                                                              | Can converge slowly and have routing loops, as well as count to infinity problem                                         |
| **Robustness**           | Each node calculates its own forwarding table. Malfunctioning nodes affect only their own data, providing robustness. | Malfunctioning nodes can advertise incorrect paths, potentially causing widespread issues and cascading failures.        |
| **Failure Scenarios**    | A router may broadcast incorrect link costs or corrupt LS packets, but route calculations remain localized.           | A router can advertise incorrect least-cost paths, potentially misleading other nodes and causing traffic flooding.      |