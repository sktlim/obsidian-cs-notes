> Connectionless transport-layer protocol where speed is critical, and reliability is not a concern

**Connectionless:** No handshake between sender and receivers

## Why use UDP at all?
- **Finer application level control over data sent**
	- TCP is subject to congestion control and resending until ACK, which might not be suitable for real time applications that require a minimum sending rate with some tolerance for data loss
- **No connection establishment**
	- UDP just blasts away
	- If reliable data transfer is required, some protocols like QUIC use UDP in the transport layer and implements reliability in its application layer
- **No connection state**
	- Servers using UDP can support more active clients as they do not need to track state
- **Small packet header overhead**
	- TCP has 20 bytes header overhead, UDP has 8 bytes

## Examples of usage
- [[Domain Name Service (DNS)]]
- SNMP
- NFS
- Streaming multimedia

## Issues with UDP
- **Lack of congestion control** potentially leads to packet overflow at routers
	- High loss rates from UDP would instead cause TCP to reduce rates
	- This responsibility is usually up to the [application](https://networkengineering.stackexchange.com/questions/79588/how-is-congestion-avoided-when-using-udp)

## UDP Segment Structure
![[UDP Segment Structure.png]]
Defined in [RFC 768](https://datatracker.ietf.org/doc/html/rfc768)

Header has 4 fields (each 2 bytes)
- Source Port
- Destination Port
- Length (length including header in bytes)
- Checksum
	- UDP at sender performs 1s complements (all 0s to 1 and all 1s to 0) of the sum of all 16-bit words in segment, with any overflow being wrapped around. Result is put in checksum
	- At receiver, all 16-bit words are added including the checksum
		- If no errors, it should produce all 1s (e.g. 1111111111111111)
		- If 1 bit is 0, means there is an error
	- UDP provides checksum as its not guaranteed that lower level layers have checksums (even though many do). Also, even after transfer, bit errors might still be introduced when segment is stored in router's memory
	- **End-end principle** in system design: functions placed at the lower levels may be redundant or of little value when compared to the cost of providing them at the higher level
	- UDP does not do anything to recover from error: it might discard damaged segment or pass segment with a warning
