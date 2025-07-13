TARGET DECK: Interview Prep::Networking::Multicast Group Membership
Q:  
What does it mean to “join” a multicast group from the host’s perspective?  
A:  
When a host joins a multicast group, it sends an IGMP Membership Report to the local router indicating interest in that group. The router then ensures multicast traffic for that group is forwarded onto the subnet. This is separate from socket-level membership and handled by the kernel.
<!--ID: 1749050753325-->


---

Q:  
What does it mean for a socket to “join” a multicast group?  
A:  
A socket joins a group using `setsockopt()` with `IP_ADD_MEMBERSHIP`. This tells the kernel to accept packets for the specified multicast address and interface. It triggers the host to send IGMP join messages if it's the first local socket to do so.
<!--ID: 1749050753328-->


---

Q:  
Can multiple sockets on the same host join the same multicast group?  
A:  
Yes. Each socket must independently join the group and bind to the same port using `SO_REUSEADDR`. Only the first join from the host sends an IGMP Membership Report to the router; subsequent joins are tracked by the OS internally.
<!--ID: 1749050753331-->


---

Q:  
What happens when all sockets leave a multicast group?  
A:  
When the last socket on a host calls `IP_DROP_MEMBERSHIP`, the kernel removes the host from the group and sends an IGMP Leave Group message to the router, prompting it to prune traffic for that group unless other hosts remain subscribed.
<!--ID: 1749050753334-->


---

Q:  
Does leaving a multicast group always stop the traffic?  
A:  
No. If another process or socket on the same host is still subscribed, traffic continues. The OS only leaves the group and stops NIC-level filtering when all group members on the host have left.
<!--ID: 1749050753337-->


---

Q:  
What happens if a process crashes without calling `IP_DROP_MEMBERSHIP`?  
A:  
The kernel will automatically clean up group memberships when the socket is closed. The host will leave the group if no other sockets are subscribed, and the router may prune the group based on the absence of IGMP reports.
<!--ID: 1749050753341-->


---

Q:  
Can a socket join the same group multiple times?  
A:  
Yes, but doing so has no additional effect. The kernel tracks the membership count and only fully leaves the group when the number of `IP_DROP_MEMBERSHIP` calls matches the number of `IP_ADD_MEMBERSHIP` calls for that socket.
<!--ID: 1749050753344-->


---

Q:  
What happens when a host joins a multicast group that it's already joined on another interface?  
A:  
The kernel treats multicast membership on a per-interface basis. Joining on one interface does not affect membership on another. To receive traffic on multiple interfaces, you must join the group separately for each interface.
<!--ID: 1749050753347-->


---

Q:  
Is there a timeout for group membership if IGMP messages are lost?  
A:  
Yes. IGMP routers periodically send Membership Queries. If a host doesn’t respond within the Max Response Time, the router assumes the host left and stops forwarding traffic. This is part of the robustness of IGMP to recover from lost leave messages.
<!--ID: 1749050753350-->


---

Q:  
Can you receive multicast traffic after leaving the group?  
A:  
No. Once the socket or host leaves the group, the kernel and NIC stop accepting traffic for that multicast address, and packets are dropped unless promiscuous or all-multicast mode is enabled.
<!--ID: 1749050753353-->
