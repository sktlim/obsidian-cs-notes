> **Node:** Any device that runs link-layer protocol
> **Link:** Communication channels that connect adjacent nodes

Over a given link, transmitting node encapsulates datagram in **link-layer frame** and transmits frame onto link

Usually implemented in a **network adapter**, such as a **Network Interface Card (NIC)**
## Services provided
- **Framing:** Encapsulation of network layer datagram
- **Link Access:** Access coordinated by Medium Access Control (MAC) protocol
- **Reliable Delivery:** Mainly for links with high bit errors, like Wifi, with goal of correcting errors locally
- **Error Detection and Correction:** Detection and correction of bit errors introduced by signal attenuation and electromagnetic noise



