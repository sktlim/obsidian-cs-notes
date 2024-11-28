> Resolves **IP address** to **MAC address**

## Properties
- ARP only resolves for hosts and interfaces on same subnet
## Process
![[ARP example.png|500]]
- If host C wants to send datagram to host A, C must give adapter both the IP datagram and the MAC address to construct link-layer frame
	- ARP module on sending host takes any IP address on same LAN as input, and returns corresponding output
	- ARP module converts IP address of A into its MAC address so that C knows what to append to the link-layer frame

Test changes

