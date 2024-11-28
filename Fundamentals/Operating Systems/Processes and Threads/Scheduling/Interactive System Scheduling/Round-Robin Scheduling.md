> Each process assigned a time interval (**quantum**) which process is allowed to run. If process still running at end of quantum, CPU is preempted and assigned to another process. 

### Implementation
- Scheduler just needs queue of ready processes
	- When process uses up quantum, its placed at back of queue

### Length of quantum
- If quantum is too short, context switching adds a lot of overhead and lowers CPU efficiency
	- e.g. context switch = 1ms, quantum = 4ms, wasted time = 20%
- If quantum too long, may cause poor response to interactive process 
- According to textbook, quantum of 20-50ms is roughly appropriate


### Advantages
1. **Fairness**: Round Robin (RR) treats each process equally by assigning a fixed time slice (or time quantum) to each one in turn.
2. **Responsive for Short Tasks**: Since each process gets to run within a fixed time quantum, Round Robin is responsive to short tasks. It often results in better performance for systems with interactive or user-facing applications, as tasks receive regular attention from the CPU.
3. **Reduced Starvation**: Unlike priority-based scheduling, Round Robin reduces the risk of starvation because every process is guaranteed CPU time at regular intervals.
4. **Simple Implementation**: RR is straightforward to implement as it only requires a queue structure to manage the processes. Each process is simply placed at the back of the queue after its time quantum expires. 

### Disadvantages
1. **High Context-Switching Overhead**: Round Robin can lead to excessive context switching, especially if the time quantum is set too low, increasing overhead.
2. **Poor Performance for Long Tasks**: Long-running processes suffer under RR because they are frequently interrupted. This can increase their total turnaround time and lead to inefficiency in completing compute-intensive jobs.
3. **Quantum Size Sensitivity**: The performance of RR heavily depends on the size of the time quantum. If it’s too short, context-switching overhead increases, degrading system performance. If it’s too long, RR starts resembling FCFS, diminishing the algorithm’s fairness and responsiveness.
4. **Not Optimal for Average Waiting Time**: Unlike algorithms like SJF, RR does not minimize average waiting time. All processes receive CPU time in cycles, so some may experience longer waits compared to more tailored algorithms.
