## Network App Architectures
- Client-server architecture
	- There is an always-on host (server) that provides services requested by other hosts (clients)
	- Ideal server has fixed , well-know IP address with multiple instances to ensure reliability
- P2P architecture
	- Minimal reliance on dedicated servers in data centers
	- App exploits direct communication between pairs of intermittently connected hosts (peers)
	- Allows for **self-scalability**
		- E.g. in a file sharing app, each peer generates workload by requesting files but at the same time adds service capacity by distributing files to other peers

## Process Communication
- In the context of a communication session between a pair of processes, the process that initiates the communication is labeled as the client
- The process that waits to be contacted to begin the session is the server
- Process sends and receives message from the network using a software interface known as the **socket**
	- **Socket** is the interface between **app process** and **transport layer protocol**

- Host is identified by IP address
- Service is identified by port number
	- Port 80: web server
	- Port 25: SMTP server

## Services provided by Transport Layer to App Layer
### Reliable Data Transfer
- To combat packet loss, a protocol can provide guaranteed data delivery, more specifically process-to-process **reliable data transfer**
- A **loss-tolerant** app does not necessarily need this service (most notably multimedia services)

### Throughput
- Protocol can provide guaranteed available throughput at certain rates
- Apps that have throughput requirements are known as **bandwidth sensitive apps** e.g. multimedia apps
- Apps that do not are known as **elastic apps** e.g. email, file transfer

### Timing
- Protocol can also provide timing guarantees
- Example guarantees
	- Every bit that sender sends into socket arrives at receiver's socket no more than 100ms later
- Useful for real-time apps

### Security
- Protocol can provide security
- Protocol can encrypt all data transmitted by sending process, and can decrypt all data before being received by receiving process
- E.g. SSL

## Transport Services Provided by Internet
### TCP Service Model
- **Connection-oriented service**
	- TCP handshake establishes TCP connection 
	- TCP connection is full-duplex connection that allows communicating processes to send messages at the same time
- **Reliable data transfer service**
	- TCP guarantees data will be sent without error and received in the proper order
- TCP also includes a congestion control mechanism which throttles a sending process

### UDP Service Model
- Lightweight transport protocol, connectionless, and unreliable data transfer
- Does not include congestion-control mechanism

Note: TCP and UDP do not provide throughput or timing guarantees, but apps have been designed to cope with this lack of guarantees


## App Layer Protocols
- App layer protocols define
	- Types of messages exchanged
	- Syntax of various message types 
	- Semantics of fields 
	- Rules for determining when and how process sends/responds to messages
- Examples
	- HTTP
	- SMTP