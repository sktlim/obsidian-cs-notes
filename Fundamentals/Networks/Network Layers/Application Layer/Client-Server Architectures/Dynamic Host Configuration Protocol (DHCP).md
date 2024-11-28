>Network management protocol that **automatically assigns IP addresses** and other configuration parameters to devices on a network

![[DHCP client and server.png|400]]
- Once organization obtains an IP address block, it assigns individual IP addresses to host and router interfaces in organization
- Sysadmin manually configures IP addresses into router
- Host addresses can also be done manually, but typically uses **DHCP**, which allows hosts to obtain IP address automatically
- Network admin can configure DHCP such that same host receives same IP address, or they may be assigned a **temporary IP address** every time they connect
- Allows host to learn a bunch of other info:
	- subnet mask
	- Address of first-hop router (default gateway)
	- Address of local DNS server
- Often referred to as **plug-and-play** or **zeroconf** protocol

- **DHCP is a client-server protocol**
	- Client is newly arriving host wanting to obtain network config info, including IP address for itself
	- In simplest case, each subnet will have its own DHCP server
	- If no server present, a DHCP relay agent that knows a DHCP server address for that network is needed 

#### DHCP Process
![[DHCP Discovery process.png|400]]
1) **DHCP Server Discovery**
	1) Newly arrived host finds DHCP server to interact by sending **DHCP discover message**, which client sends within UDP packet to port 67 along with broadcast destination 255.255.255.255 and a "this host" source IP of 0.0.0.0 
	2) DHCP Client passes IP datagram to link layer, which broadcasts frame to all nodes attached to subnet
2) **DHCP Server Offers**
	1) DHCP server receiving discover message responds to client with **DHCP offer message** that is broadcast to all nodes on subnet
		1) Broadcast is used because client does not have IP address yet
		2) Each offer message contains:
			1) transaction ID of discover message
			2) proposed IP address
			3) network mask
			4) **IP address lease time**: Amount of time IP address will be valid
3) **DHCP Request**
	1) Client will choose one of the servers offered and respond using a **DHCP Request Message**, echoing back config params
4) **DHCP ACK**
	1) Server responds to request message with a **DHCP ACK** confirming the params

- DHCP also provides a mechanism for IP address lease renewal