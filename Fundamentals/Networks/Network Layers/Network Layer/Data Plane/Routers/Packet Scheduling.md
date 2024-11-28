## FCFS
![[FCFS Packet Scheduling.png]]
## Priority Queueing
![[Priority Queueing packet scheduling.png]]
- Queue may be configured so that packets carrying network management information (e.g., as indicated by the source or destination TCP/UDP port number) receive priority over user traffic; 
- Real-time voice-over-IP packets might receive priority over non-real traffic such as SMTP or IMAP e-mail packets

## Round Robin and Weighted Fair Queueing (WFQ)
![[WFQ packet scheduling.png]]
- **Work-conserving queuing** discipline will never allow the link to remain idle whenever there are packets (of any class) queued for transmission. 
- **Work-conserving round robin discipline**: If it looks for a packet of a given class but finds none  it will immediately check the next class in the round robin sequence.
- **WFQ:** Differs from RR as each class may receive differential amount of service in any time interval
	- Each class $i$ is assigned a weight $w_i$
	- During any time interval where class $i$ has packets to send, it will get service equal to $\frac{w_{i}}{\\\sum w_{j}}$
	- In worst case, bottom summation is over all classes
	- For link with transmission $R$, throughput will be at least $R*\frac{w_{i}}{\\\sum w_{j}}$