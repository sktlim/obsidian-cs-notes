### Categories
- **Hard real-time:** Absolute deadlines
- **Soft real-time:** Missing occasional deadline is ok

- Real time behavior achieved by dividing program into number of processes, each of whose behavior is well-known and predictable 

- **Periodic:** Occur at regular intervals
- **Aperiodic:** Occur unpredictably

### RTOS properties
Handling all processes may not be possible, and load can only be handled if $$\sum_{i=1}^{m} \frac{C_i}{P_i} \leq 1$$ where:
	$m$ = number of periodic events
	$i$ = event count
	$P$ = period
	$C$ = seconds of CPU time to handle each event

Real-time system that meets this criterion is said to be schedulable. Calculation assumes that context switch overhead can be ignored

### Separation of Scheduling Mechanism and Scheduling Policy
- **Scheduling Mechanism**: Methods for implementing policy
- **Scheduling Policy:** Deciding which process should run
- Scheduling algorithms are parametrized such that the params can only be filled up by user processes
- Basically allows user processes to set scheduling policies according to their needs, and allow kernel to enforce these policies through its scheduling mechanisms
- The idea of user threads and processes fit this model
