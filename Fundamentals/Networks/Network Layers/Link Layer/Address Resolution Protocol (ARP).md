> Resolves **IP address** to **MAC address**

## Properties
- ARP only resolves for hosts and interfaces on same subnet
- Each host and router has an **ARP table** in its memory, containing mappings of IP addresses to MAC addresses
## Process
![[ARP example.png|500]]
- If host C wants to send datagram to host A, C must give adapter both the IP datagram and the MAC address to construct link-layer frame
	- ARP module on sending host takes any IP address on same LAN as input, and returns corresponding output
	- ARP module converts IP address of A into its MAC address so that C knows what to append to the link-layer frame

- If C wants to send datagram to A, but C's ARP table doesn't have destination address, C needs to use the ARP protocol to resolve the address
	- C constructs an **ARP packet** containing sending and receiving IP and MAC addresses
	- C passes an **ARP query packet** to adapter and indicates that adapter should send it to MAC broadcast address FF-FF-FF-FF-FF-FF
	- Adapter encapsulates ARP packet in link-layer frame, uses broadcast address for frame's destination address, and transmits frame into subnet
	- Each adapter on the subnet receives broadcast and passes ARP packet up to its ARP module, where the ARP module then checks to see if its IP matches the destination IP in the ARP packet
	- ARP matching destination IP sends back to querying host a response ARP packet with desired mapping
	- Querying host C updates it ARP table and sends its IP datagram encapsulated in link-layer frame with destination MAC address being the host/router that just responded to the earlier query

