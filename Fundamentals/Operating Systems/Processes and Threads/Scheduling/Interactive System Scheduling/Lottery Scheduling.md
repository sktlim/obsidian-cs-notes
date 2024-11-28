> Gamble for CPU time

### Key concepts
- Gives processes lottery tickets for various system resources
- Scheduler picks random lottery ticket, and process holding it gets the resource
- When applied for CPU scheduling, OS might hold a lottery every 20ms
- **Priority**
	- More important processes can be given extra tickets
	- Clearer than priority scheduling, as holding 20 tickets out of 100 means 20% chance of running

### Advantages
- **Highly responsive:** New process has chance of winning lottery if granted sufficient tickets
- **Processes can cooperate:** If client process sends message to server process and blocks, it can give all its tickets to server process

### Disadvantages
1. **Unpredictability**: Due to its randomized nature, lottery scheduling can lead to unpredictable response times, which may not be suitable for real-time or latency-sensitive systems.
2. **Complexity in Ticket Allocation**: Managing and balancing tickets across processes can be complex, especially in systems with varying workloads or user priorities.