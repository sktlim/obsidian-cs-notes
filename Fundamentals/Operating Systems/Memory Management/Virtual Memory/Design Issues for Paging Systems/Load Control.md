> When combined working set of all processes exceed capacity of memory, system thrashes

Symptom: When PFF indicates some processes need more memory, but no processes need less memory

## Solution
### Simple Approach
- Kill some processes
- OS have special process called **Out Of Memory killer (OOM)** that activates when system runs low on memory
	- OOM activates when system runs low on memory, and reviews all running processes to select something to kill based on a badness score (how much memory it takes up)

### Friendly Approach
- Swap some processes to non-volatile storage and free up the page frames being held
- Pages are swapped out until thrashing stops
- Similar to two-level scheduling

### Other approaches
- Compaction and compression
- Deduplication/same page merging
	- Sweep memory to see if 2 pages have exact same content
	- virtual pages then point to same virtual frame; frame is shared copy-on-write, so if process writes to page, fresh copy is made