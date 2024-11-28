>Unique, fixed-length identifier attached to a **network interface controller (NIC)** for communication on local network, consisting of 48-bits (6-bytes) typically represented in hexadecimal format

## Properties 
- 6 bytes give $2^{48}$ possible addresses
- IEEE manages address space for MAC addresses; when a company wants to manufacture adapters, they purchase a $2^{24}$ chunk of addresses (first 24 bits)

## Sending Frames Across Adapters
- When an adapter wants to send a frame, the sending adapter inserts the destination adapter's MAC address into the frame and then send the frame into the LAN
- 