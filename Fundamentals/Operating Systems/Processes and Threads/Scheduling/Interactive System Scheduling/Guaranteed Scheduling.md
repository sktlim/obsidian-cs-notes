> Make real promises to users about performance and fulfill them. 

### Properties
- OS must track how much CPU each process had since creation
- Then computes amount of CPU each process is entitled to
- Ratio of CPU used to CPU entitled is calculated
- Process with lowest ratio is preemptively ran, until its ratio increases to the point where the next process preempts it

- Linux's **Completely Fair Scheduling (CFS)** runs a variant of this where it keeps track of "spent execution time" in a red-black tree
	- Left-most node corresponds to process with least spent execution time
	- Scheduler indexes tree by execution time and selects left-most node to run
	- When process stops, scheduler reinserts process into tree 

### Advantages
1. **Fair Resource Allocation**: Each process is guaranteed a fair share of CPU time, making it suitable for multi-user systems where equitable distribution of resources is important.
2. **Predictable Performance**: Guaranteed scheduling provides predictable performance, as users know in advance the minimum CPU time they can expect, enhancing user experience.
3. **Avoidance of Starvation**: By providing each process with a guaranteed share, the algorithm prevents starvation, ensuring that every process receives CPU time.

### Disadvantages
1. **Complex Implementation**: Implementing guaranteed scheduling requires tracking CPU usage for each process and calculating entitlements, adding overhead and complexity to the scheduler.
2. **Less Optimal for High-Performance Systems**: Since it focuses on fairness over efficiency, guaranteed scheduling may not maximize CPU utilization, especially if some processes don’t fully utilize their allocated share.
3. **Difficulty in Handling Variable Loads**: In systems with highly variable workloads, meeting each process's guaranteed CPU share can be challenging, potentially impacting overall performance and responsiveness.