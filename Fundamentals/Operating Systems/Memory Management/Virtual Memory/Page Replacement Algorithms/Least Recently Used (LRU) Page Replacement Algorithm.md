> **Least Recently Used (LRU) Page Replacement Algorithm:** Replace page that has not been used for the longest time

## Implementation
### Dedicated Hardware: Simplest
- Hardware with 64-bit counter *C* that automatically increments after each instruction
- Page table entry must have field with enough space for counter

- After each memory reference, value of *C* is stored in page table entry for page just referenced
- When page fault occurs, OS examines all page counters in table to find the lowest

### Software
#### Not Frequently Used (NFU) Algorithm
- Software counter associated with each page, initially 0
- At each clock interrupt, OS scans memory for each page, and the value of *R* is added to counter
- Counter roughly keeps track how often each page is referenced and on page fault, replaces the page with the lowest value

- **Issue:** Pages with high count from previous usages remain high even if they might not be used anymore
- **Solution:** Aging
	- ![[Aging Example.png]]
	- Counters are shifted right by 1 bit before current *R* bit is added
	- **Difference between Aging and LRU**
		- Aging has limited time horizon as it has finite number of bits while LRU stores complete history
