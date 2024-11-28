| Feature                 | Circuit Switching                                                                    | Packet Switching                                                                   |
| ----------------------- | ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| **Connection**          | Requires a dedicated connection established before communication starts.             | No dedicated connection; data is sent as independent packets.                      |
| **Resource Allocation** | Resources (bandwidth) are reserved for the entire session.                           | Resources are used dynamically, shared among multiple users.                       |
| **Efficiency**          | Inefficient if the channel is underutilized (e.g., during silence in a call).        | Highly efficient as network paths are shared among many users.                     |
| **Reliability**         | Guaranteed connection quality and bandwidth.                                         | Variable quality due to shared paths and possible congestion.                      |
| **Latency**             | Low latency once the circuit is established.                                         | Higher latency due to packet routing and potential queuing.                        |
| **Data Transfer**       | Continuous and steady data flow.                                                     | Data is sent in packets that may arrive out of order or get lost.                  |
| **Setup Time**          | Requires time to establish the circuit before communication.                         | No setup time; data can be sent immediately.                                       |
| **Cost**                | Higher due to dedicated resources per connection.                                    | Lower due to shared resources and dynamic allocation.                              |
| **Use Cases**           | Best for real-time, continuous communication like voice calls or video conferencing. | Ideal for data-centric applications like emails, file transfers, and web browsing. |
| **Example Networks**    | Traditional telephone networks (PSTN).                                               | Internet and modern data networks (TCP/IP).                                        |

### Summary
- **Circuit Switching** is better for real-time, continuous communication with predictable quality but is less efficient.
- **Packet Switching** is more flexible, scalable, and efficient, making it the backbone of modern data networks like the internet.

### Example of Packet Switching Efficiency
- **Initial assumptions**
	- Users share 1Mbps link 
	- User alternates between periods of activity and inactivity
		- Activity: Generates data at 100 kbps
		- No activity: No data generated
	- User active only 10% of time
- **Circuit switching**
	- 100 kbps must be reserved for each user at all times
	- Using TDM with 10 slots of 100ms each, each user will have one slot per frame
	- Circuit switch can only support 10 users
- **Packet Switching**
	- Probability user is active = 0.1
	- If there are 35 users, chance that 11 or more simultaneously active is 0.0004
		- Assuming user activity follows binomial distribution, $P(X\geq 11) = 0.00042$
	- When there are 10 or less users, total generated traffic is less than or equals to 1Mbps and packets flow through link without delay
	- If there are 11 or more, then output queue will begin to grow until aggregate input rate falls below 1Mbps
	- Packet switching allows for x3 users while giving essentially same performance as circuit switching

### Another Example
- **Initial Assumptions**
	- 10 users, with 1 user suddenly generating 1000 x 1000 bit packets while other users remain quiescent (inactive)
- **Circuit switching**
	- Assuming 10 slots per frame with each slot consisting of 1000 bits and 100ms,
	- Only 1 slot is active per frame as user can only use that much
	- 10 seconds before data transmitted
- **Packet Switching**
	- User can continuously transmit at 1Mbps (as no other users transmitting data) and will complete within 1 second