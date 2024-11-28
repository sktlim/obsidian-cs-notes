### Context switches
- Switch from user mode to kernel mode
- State of current process must be saved, including storing registers in process table so that it can be reloaded later
	- In some systems, memory map must be saved as well
- New process must be selected by scheduler
- **Memory Management Unit (MMU)** must be reloaded with memory map of new process
- New process is started

- Context switch may also invalidate memory cache and related tables, forcing it to be dynamically reloaded from memory

### Process Behavior
- **Compute-bound/ CPU-bound:** Processes that spend most of their time computing
	- Long CPU burst, infrequent IO waits
- **I/O-bound:** Processes that spend most of their time waiting for I/O
	- Frequent IO waits, short CPU bursts 
- Key factor is length of CPU burst, not IO
	- As CPUs get faster, processes become more IO bound

### When to schedule
- When new process is created
- When process exits
- When process blocks
- When IO interrupts occurs
- When hardware clock periodically interrupts

### Scheduling Algorithms
- **Non-preemptive:** Picks process and lets it run until it blocks or voluntarily releases CPU
- **Preemptive**: Picks process and lets it run some maximum fixed time

### Scheduling Goals
- All systems
	- **Fairness**: giving each process a fair share of the CPU
	- **Policy enforcement**: seeing that stated policy is carried out
	- **Balance**: keeping all parts of the system busy
- Batch systems
	- **Throughput:** maximize jobs per hour
	- **Turnaround time:** minimize time between submission and termination
	- **CPU utilization:** keep the CPU busy all the time
- Interactive systems
	- **Response time:** respond to requests quickly
	- **Proportionality:** meet users’ expectations
- Real-time systems
	- **Meeting deadlines:** avoid losing data
	- **Predictability:** avoid quality degradation in multimedia systems

### Scheduling in 
- Batch systems
	- [[First come first serve (FCFS)]]
	- [[Shortest Job First (SJF)]]
	- [[Shortest Remaining Time First (SRTF)]]
- Interactive Systems
	- [[Round-Robin Scheduling]]
	- [[Priority Scheduling]]
	- [[Shortest Job Next (SJN)]]
	- [[Fair-Share Scheduling]]
	- [[Guaranteed Scheduling]]
	- [[Lottery Scheduling]]
- Real time systems
	- 