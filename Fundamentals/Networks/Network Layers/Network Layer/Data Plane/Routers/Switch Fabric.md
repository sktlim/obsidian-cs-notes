- Medium through which packets are switched from input to output port
## Switching via Memory 
![[Switch fabric memory.png]]
- Earliest routers were normal computers
- Input and output ports functioned as traditional I/O devices in traditional OS

### Process
- Input port with arriving packet first signaled the processor using interrupt
- Packet copied from input port into processor memory
- Routing processor extracted destination address from header, looked up appropriate output port, and copied packet into output's buffer
- If memory bandwidth is $B$, overall forwarding throughput is $\leq B/2$ 
- Packets can't be forwarded at same time since only one R/W can be done over shared system bus

Some modern routers switch using memory, with a major difference being lookup of destination address and storing of packet into appropriate memory location performed by processing on input line cards

## Switching via Bus
![[Switch fabric bus.png]]
- Input port directly transfers packet onto output port over shared bus without intervention from routing processor
- Typically done by having input port prepend a switch-internal header value indicating outgoing port 
- Packet sent to all outgoing links, and discarded in all but the correct outgoing port, where it will remove the prepended header value
- Only one packet can cross bus at a time, but this is sufficient for small LANs and enterprise nets

## Switching via Interconnection Network
![[Switch fabric crossbar.png]]
- To overcome bandwidth limitation of bus
- Crossbar switch is an interconnection network consisting of $2N$ buses that connect $N$ input ports to $N$ output ports
- Each vertical bus intersects a horizontal bus at a crosspoint, which can be opened and closed at anytime by the switch fabric controller 
### Process
- When packet arrives from A and needed to be forwarded to Y, switch controller closes crosspoint at intersections of A and Y 
- Packet from B to X can be forwarded at the same time as it they don't have the same input and output ports
- Crossbar switch is non-blocking as long as packets do not have same output port

