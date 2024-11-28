> Each process assigned a priority, and process with highest priority runs

## Properties
- **Preventing Starvation**
	- To prevent high priority processes from running indefinitely and causing starvation, scheduler may decrease priority of the running process at each clock tick
		- Context switch occurs if priority of running process drops below that of highest priority process
	- Alternatively, each process may be assigned a max time quantum to run
		- Once quantum runs up, context switch happens
		- After some idle time, another algorithm re-assigns a priority to the process to prevent starvation
- **Priority Assignment**
	- Priorities can be assigned statically or dynamically
		- Statically assigned according to role 
			- e.g. student gets priority 20, teacher 50
		- Dynamically assigned according to system goals
			- e.g. IO bound processes typically complete quickly and should be assigned CPU when possible. Priority is therefore set as 1/f where f is fraction of last quantum used. IO bound processes will have higher priority as compared to CPU bound processes
- Often convenient to group processes into **priority groups** with round robin scheduling in each group

### Advantages
1. **Efficient for Critical Tasks**: High-priority tasks are processed quickly.
2. **Flexible Priority Assignment**: Allows static or dynamic priority settings based on system needs.
3. **Improves Response for Key Tasks**: Reduces wait times for important, time-sensitive tasks.

### Disadvantages
1. **Starvation Risk**: Low-priority tasks may wait indefinitely if high-priority tasks keep arriving.
2. **Complex to Manage**: Dynamic priority adjustments add implementation overhead.
3. **Inconsistent Waiting Times**: Lower-priority tasks may experience long, unpredictable delays.
4. **High Context Switching**: Frequent high-priority tasks increase context-switching, lowering efficiency.