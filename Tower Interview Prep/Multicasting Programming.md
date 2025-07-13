TARGET DECK: Interview Prep::Networking::Multicast Programming
Q:  
How do you create a socket to receive multicast traffic in C++?  
A:  
You create a UDP socket using `socket(AF_INET, SOCK_DGRAM, 0)`, then bind it to the multicast port using `bind()`. After that, you join the multicast group using `setsockopt()` with the `IP_ADD_MEMBERSHIP` option, specifying the group IP and interface.
<!--ID: 1749050660945-->


---

Q:  
What does `IP_ADD_MEMBERSHIP` do in socket programming?  
A:  
`IP_ADD_MEMBERSHIP` is a socket option that tells the operating system to join a multicast group. This enables the socket to receive datagrams addressed to the specified multicast IP, and it configures the NIC to accept packets for that group.
<!--ID: 1749050660948-->


---

Q:  
Why do you bind a multicast socket to `INADDR_ANY`?  
A:  
Binding to `INADDR_ANY` allows the socket to receive multicast datagrams sent to any local interface. This is important for flexibility, especially on machines with multiple network interfaces, as it lets the OS route multicast packets from any interface to the socket.
<!--ID: 1749050660951-->


---

Q:  
How do you leave a multicast group in C++?  
A:  
You call `setsockopt()` with the `IP_DROP_MEMBERSHIP` option, passing the same `ip_mreq` structure used to join the group. This tells the kernel and NIC to stop receiving packets for that multicast group.
<!--ID: 1749050660955-->


---

Q:  
What does `IP_MULTICAST_LOOP` control?  
A:  
`IP_MULTICAST_LOOP` controls whether packets sent by the local process to a multicast group are looped back and received by the same host. Setting it to 0 disables loopback, while 1 enables it. This is useful when testing or simulating multicast locally.
<!--ID: 1749050660958-->


---

Q:  
What is `IP_MULTICAST_TTL` used for?  
A:  
This socket option sets the TTL (Time To Live) value for outbound multicast packets. It controls how many routers the packet can pass through. A value of 1 keeps the packet local to the subnet, which is often desirable for low-latency environments like HFT.
<!--ID: 1749050660962-->


---

Q:  
How do you choose the outgoing interface for multicast in C++?  
A:  
Use the `IP_MULTICAST_IF` socket option with `setsockopt()`, passing in the local IP address of the interface you want to use. This ensures outbound multicast packets are sent from the correct NIC in multi-interface systems.
<!--ID: 1749050660965-->


---

Q:  
Can multiple sockets join the same multicast group and port?  
A:  
Yes, but you must set the `SO_REUSEADDR` option before binding. This allows multiple sockets to bind to the same port. Each socket that joins the same multicast group will receive a copy of the packet independently.
<!--ID: 1749050660968-->


---

Q:  
What happens if you don't set `SO_REUSEADDR` before binding a multicast socket?  
A:  
If another socket is already bound to the same port, your `bind()` call will fail with an "Address already in use" error. `SO_REUSEADDR` allows multiple sockets to coexist on the same port for multicast reception.
<!--ID: 1749050660971-->


---

Q:  
How can you send multicast packets in C++?  
A:  
Create a UDP socket and use `sendto()` with a destination address in the multicast range (e.g., 239.x.x.x). Make sure to set `IP_MULTICAST_IF` to choose the sending interface and optionally configure `IP_MULTICAST_TTL` and `IP_MULTICAST_LOOP`.
<!--ID: 1749050660976-->


Q:  
What is the difference between `SO_REUSEADDR` and `SO_REUSEPORT` in multicast sockets?  
A:  
`SO_REUSEADDR` allows multiple sockets to bind to the same address and port combination, typically required for multicast. `SO_REUSEPORT`, available on some platforms, allows multiple sockets to accept datagrams in parallel, enabling load balancing across threads or processes. Both may be needed depending on the OS.
<!--ID: 1749050660980-->


---

Q:  
What happens if you bind a multicast socket to the multicast IP instead of `INADDR_ANY`?  
A:  
Binding to the multicast IP can prevent the socket from receiving any packets because the kernel expects unicast bindings unless explicitly configured otherwise. The standard approach is to bind to `INADDR_ANY` and use `IP_ADD_MEMBERSHIP` to listen for a specific group.
<!--ID: 1749050660983-->


---

Q:  
How do you bind to a specific interface for receiving multicast?  
A:  
You usually bind to `INADDR_ANY` but specify the interface in the `ip_mreq` struct's `imr_interface` field when using `IP_ADD_MEMBERSHIP`. This tells the OS which interface to use for listening to that multicast group.
<!--ID: 1749050660987-->


---

Q:  
What is the structure of `ip_mreq` used in multicast socket programming?  
A:  
`ip_mreq` contains two fields: `imr_multiaddr`, the multicast group IP to join, and `imr_interface`, the local interface IP (or `INADDR_ANY`) to receive on. It’s passed to `setsockopt()` when joining or leaving multicast groups.
<!--ID: 1749050660991-->


---

Q:  
Can you join multiple multicast groups on a single socket?  
A:  
Yes, you can call `setsockopt(IP_ADD_MEMBERSHIP)` multiple times on the same socket with different `ip_mreq` structs. The socket will then receive packets from all specified groups.
<!--ID: 1749050660994-->


---

Q:  
What is the limitation of the `ip_mreq` struct regarding interface selection?  
A:  
`ip_mreq` uses an IP address to specify the interface, which can be ambiguous if multiple interfaces share the same IP or if the address changes. To handle this more reliably on newer systems, use `ip_mreqn`, which allows specifying the interface by index.
<!--ID: 1749050660998-->


---

Q:  
What happens if two sockets on the same host join the same multicast group on the same port?  
A:  
Both sockets will receive a copy of each incoming packet, as long as they’ve correctly set `SO_REUSEADDR` and joined the group. This is useful for debugging or processing the same data in parallel.
<!--ID: 1749050661003-->


---

Q:  
What is the maximum number of multicast groups a socket or host can join?  
A:  
The number is system-dependent. For Linux, the limit is defined by `net.ipv4.igmp_max_memberships` (default is 20). You can check and adjust this via `sysctl` if needed for applications like market data feeds that require subscribing to many groups.
<!--ID: 1749050661006-->


---

Q:  
Can you receive multicast traffic without joining a group?  
A:  
No. Unless the NIC is in promiscuous or all-multicast mode, you must explicitly join the group using `IP_ADD_MEMBERSHIP` to receive the packets. Otherwise, the OS and NIC will discard them.
<!--ID: 1749050661011-->


---

Q:  
What is a typical reason you might not receive multicast packets despite joining the group?  
A:  
Common issues include incorrect interface IP in `ip_mreq`, lack of `SO_REUSEADDR` before bind, mismatched port, multicast not being forwarded on the switch/router, or the NIC not being configured to accept the multicast MAC.
<!--ID: 1749050661015-->
