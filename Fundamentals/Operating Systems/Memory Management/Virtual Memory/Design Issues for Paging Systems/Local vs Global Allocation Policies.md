> How should memory be allocated to competing runnable processes?

## Example
![[local versus global allocation.png|500]]
- **Local Replacement:** If lowest age of process A chosen, **A5** will be chosen
	- Akin to allocating a fixed portion of memory to each process
- **Global Replacement:** If lowest age chosen regardless of process, **B3** will be chosen for replacement
	- Dynamic allocation of page frames
### Analysis
- Global algorithms work better, especially when working set size varies a lot during lifetime of a process
- If local algorithm used and 
	- working set grows, thrashing will occur even if there are sufficient number of free page frames
	- working set shrinks, there will be wasted memory 

## Alternative approach: Algorithm for page frame allocation
- **Preliminary idea:** Periodically sample number of running processes and equally divide total page frames among them
	- E.g. 10 processes, 12,000 page frames = 1200 page frames per process
- Might not be fair as some processes might address more things. Quick modification: page frames should be divided by total size of process, with a minimum number in place to ensure program can run
- Allocation must be updated dynamically. This can be done with the **Page Fault Frequency Algorithm**
### Page Fault Frequency (PFF) Algorithm
- ![[Page fault frequency.png|400]]
- Assumed that fault rate decreases for more page frames assigned
	- Point A: Fault rate too high, need more page frames
	- Point B: Fault rate too low, process too much memory
- Number of faults per second recorded, with moving average taken
- PFF keeps page fault rate between A and B

## Final Note
- Some page replacement algorithms can either be global or local
	- E.g. LRU, FIFO
- Other replacement algorithms only make sense in local context
	- E.g. working set, WSClock (leverage on principle of locality)