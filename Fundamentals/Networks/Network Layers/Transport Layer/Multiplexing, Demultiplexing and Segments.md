> **Demultiplexing:** Deliver data in transport-layer segments to the correct socket
> **Multiplexing:** Gathering data chunks from different sockets, encapsulating each chunk with header info, passing segments to network layer

## What is in a Transport Layer Segment?
![[Transport Layer Segment.png]]
- Port numbers range from 0 to 65535
- Port number that range from 0 to 1023 are **well-known port numbers** and usage is restricted (reserved for well-known protocols)
- Given in RFC 1700

## UDP Multiplexing/Demultiplexing 
- UDP socket is fully identified by tuple `(destination_IP, destination_port)`
- Source port number serves as return address

- In essence for UDP, to ensure proper multiplexing/ demultiplexing, applications need to specify the destination IP and destination port address

## TCP Multiplexing/Demultiplexing
![[TCP Demultiplexing.png]]
- TCP socket identified by 4-tuple `(source_IP, source_port, destination_IP, destination_port)`
- In contrast with UDP, 2 TCP segments with different source port/IP will be directed to 2 different sockets
- **Note** that there is not always 1-1 correspondence between connection sockets and processes
- Non-persistent HTTP can also hurt performance of a busy web server