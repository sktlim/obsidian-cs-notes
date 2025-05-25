- **Demand Paging:** Pages are loaded based on demand from process. Page table is initially empty and will page fault to load in first entry, and so on
- **Locality of reference:** Processes reference only a relatively small number of pages
- If page fault occurs, OS must choose page to evict from page table
	- If page has been modified while in memory, it must be rewritten to non-volatile storage
	- Else, no rewrite needed
- System performance better if not frequently used page is evicted

## Theoretical best Page Replacement Algorithm
- Each page labeled with number of instructions before page is first referenced
- Page with most number of instructions is removed

- Cannot be done *on first run*. At time of page fault, OS has no way of knowing when each of pages will be referenced next
- Possible on second run using the page reference info from when program runs again for the second time

- Typically used for measuring performance of optimal algorithm for comparison with real-world algorithms
## Real-world Algorithms
- [Not Recently Used Page Replacement Algorithm]