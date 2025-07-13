TARGET DECK: Interview Prep::Networking::Multicast

Q:  
What is multicast in networking?
A:  
Multicast is a method of sending network packets from one sender to **multiple specific receivers** efficiently. Instead of duplicating packets for each recipient like unicast, the sender sends one packet to a **multicast group address**, and routers/switches replicate it only when needed, reducing bandwidth usage.
<!--ID: 1749050436948-->


Q:  
How is multicast different from unicast and broadcast?
A:
- Unicast: one-to-one (each packet goes to one host)
- Broadcast: one-to-all (sent to every host in a network)
- Multicast: one-to-many (sent to only subscribed members of a multicast group). Multicast avoids the inefficiency of broadcast and the overhead of multiple unicast streams.

---

Q:  
What IP address range is reserved for multicast?
A:  
Multicast IPs use **Class D** addresses, ranging from **224.0.0.0 to 239.255.255.255**. These are reserved specifically for multicast traffic and are not used for normal unicast routing.
<!--ID: 1749050436955-->


---

Q:  
What is the significance of the 224.0.0.x address block?
A:  
The 224.0.0.x range is reserved for **link-local multicast** (used only within a local network segment). Routers never forward these packets, which makes them useful for routing protocols and discovery services like OSPF or mDNS.
<!--ID: 1749050436958-->


---

Q:  
What is a multicast group?
A:  
A multicast group is identified by a single IP address in the Class D range. Hosts can **join or leave** the group dynamically using protocols like IGMP. Any packet sent to the group's IP is received by all members who have joined that group on the same subnet.
<!--ID: 1749050436961-->


---

Q:  
Can multiple applications on the same host join the same multicast group?
A:  
Yes. Multiple sockets on the same machine can join the same multicast group and receive the same traffic. Each socket will independently receive a copy of the multicast packet.
<!--ID: 1749050436964-->


---

Q:  
What is Source-Specific Multicast (SSM)?
A:  
SSM is a form of multicast where receivers specify **both the multicast group and the source IP** they want to receive traffic from. This allows for better control and security since only specific sources are trusted.
<!--ID: 1749050436967-->


---

Q:  
What are administratively scoped multicast addresses?
A:  
These are multicast IP ranges reserved for internal/private network use (similar to private IPs for unicast). The most common range is **239.0.0.0 to 239.255.255.255**. Routers are configured not to forward these beyond administrative boundaries.
<!--ID: 1749050436973-->


---

Q:  
Why are multicast addresses not routable over the public internet?
A:  
Multicast routing is not supported across the public internet because it requires complex coordination between routers and group management. Instead, multicast is used within **controlled environments like data centers or financial networks**, where routing and group membership can be managed explicitly.
<!--ID: 1749050436976-->


---

Q:  
How does a device know which multicast traffic to accept?
A:  
A device (or its NIC) only accepts multicast traffic for groups it has **explicitly joined** via `setsockopt()` in the OS or via IGMP at the network level. NICs use **hardware filtering** to drop unwanted multicast traffic early, reducing CPU load.
<!--ID: 1749050436979-->


Q:  
What does it mean for a host to "join" a multicast group?
A:  
When a host joins a multicast group, it tells the network (via IGMP) that it wants to receive traffic addressed to a specific multicast IP. This enables routers and switches to forward the group’s traffic to that host’s subnet and, optionally, filter it to just the interested devices.
<!--ID: 1749050436982-->


---

Q:  
What does it mean for a socket to "join" a multicast group?
A:  
At the application level, a socket joins a multicast group by using `setsockopt()` with the `IP_ADD_MEMBERSHIP` option. This instructs the OS to allow the socket to receive packets for a specified multicast group IP, assuming the host has also joined it at the network level.
<!--ID: 1749050436985-->


---

Q:  
What protocol is typically used for delivering multicast traffic?
A:  
Multicast traffic is usually delivered using **UDP**, a connectionless, best-effort transport protocol. It doesn't guarantee delivery, order, or duplication protection, which keeps latency low — ideal for high-speed environments like HFT.
<!--ID: 1749050436988-->


---

Q:  
What is the TTL (Time To Live) in multicast, and why is it important?
A:  
TTL is a field in the IP header that specifies how many router hops a multicast packet can make before it’s dropped. This prevents packets from circulating indefinitely. You can control TTL using `IP_MULTICAST_TTL` in your socket code.
<!--ID: 1749050436991-->


Q:  
How is a multicast IP address translated to a MAC address?
A:  
IPv4 multicast addresses are mapped to Ethernet MAC addresses by taking the lower 23 bits of the IP address and prefixing them with 01:00:5e. For example, the IP 224.1.1.1 maps to the MAC 01:00:5e:01:01:01. This mapping means different multicast groups can sometimes share the same MAC, causing the NIC to receive traffic for multiple groups.
<!--ID: 1749050436994-->


Q:  
How is a multicast IP address translated to a MAC address?
A:  
IPv4 multicast addresses are mapped to Ethernet MAC addresses by taking the lower 23 bits of the IP address and prefixing them with 01:00:5e. For example, the IP 224.1.1.1 maps to the MAC 01:00:5e:01:01:01. This mapping means different multicast groups can sometimes share the same MAC, causing the NIC to receive traffic for multiple groups.
<!--ID: 1749050661019-->


---

Q:  
How does Ethernet handle multicast traffic?
A:  
Ethernet treats multicast as a special class of broadcast. Frames with MAC addresses starting with 01:00:5e are considered multicast and are delivered to all ports unless IGMP snooping is enabled. Switches may flood multicast traffic like broadcast unless they're configured to constrain it.
<!--ID: 1749050661022-->


---

Q:  
What is the role of the NIC in filtering multicast traffic?
A:  
Network Interface Cards (NICs) support hardware filtering of multicast addresses. When a socket joins a multicast group, the OS configures the NIC to listen only for the corresponding MAC address. This minimizes CPU overhead by dropping irrelevant multicast packets in hardware.
<!--ID: 1749050661025-->


---

Q:  
What is promiscuous mode and how does it relate to multicast?
A:  
Promiscuous mode is when a NIC receives all traffic regardless of destination MAC. It is sometimes used for debugging or monitoring. In contrast, multicast-capable NICs selectively listen to only multicast MACs they’ve been programmed for, unless promiscuous mode is enabled.
<!--ID: 1749050661029-->


---

Q:  
What happens if multiple multicast IPs map to the same MAC address?
A:  
Since only 23 bits of the IP address are used in mapping to the MAC, collisions can occur where different IP multicast groups resolve to the same MAC. This can cause the NIC to receive unwanted packets for groups it hasn’t joined, leading to potential performance overhead.
<!--ID: 1749050661033-->


---

Q:  
What is the significance of 224.0.0.x addresses in Ethernet?
A:  
Multicast IPs in the 224.0.0.x range are considered link-local and are not forwarded by routers. Ethernet switches treat them like local broadcast messages, delivering them to all hosts on the segment. They're used by routing protocols like OSPF and should be filtered carefully in production networks.
<!--ID: 1749050661036-->


---

Q:  
How does TTL affect multicast packet propagation?
A:  
TTL, or Time To Live, limits how far a multicast packet can travel. Each router that forwards the packet decrements the TTL. When TTL reaches zero, the packet is dropped. This controls the scope of delivery and helps prevent routing loops.
<!--ID: 1749050661039-->


---

Q:  
How can you control the interface that sends multicast packets?
A:  
Using the `IP_MULTICAST_IF` socket option in C++, you can specify the outbound interface for multicast packets. This is important on multi-homed systems to ensure the packets go out on the correct NIC.
<!--ID: 1749050661043-->


---

Q:  
Do multicast packets use ARP for address resolution?
A:  
No. ARP is used for resolving unicast IPs to MACs. For multicast, the IP is algorithmically mapped to a MAC address, and no ARP request is sent. The host simply sends the frame to the derived multicast MAC.
<!--ID: 1749050661047-->


---

Q:  
How does Ethernet switching affect multicast performance?
A:  
Without IGMP snooping, switches flood multicast traffic to all ports like broadcast, which wastes bandwidth and processing. With IGMP snooping, switches learn which ports need which multicast groups and only forward packets accordingly, improving efficiency.
<!--ID: 1749050661050-->


