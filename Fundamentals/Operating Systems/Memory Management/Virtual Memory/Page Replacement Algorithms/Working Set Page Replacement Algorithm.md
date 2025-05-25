> **Working Set Page Replacement Algorithm:** 

## Terminology
- **Working Set:** Set of pages process is currently using
- **Thrashing:** Program causing page fault every few instructions

## Intuition
- When process is loaded from disk to run on CPU, it thrashes until it get its working set which wastes CPU time
- Paging systems try to keep track of a process' working set and load it in when process runs, which forms the basis of the **working set model**
	- Loading pages before process runs is known as **prepaging**
## Working Set Model
![[Working set model.png|400]]
- At time $t$, there is a set of all pages used by $k$ most recent memory references, which is the working set $w(k,t)$
	- $k>1$ most recent references must have used all the pages used by $k=1$ and possibly others. Hence, $w(k,t)$ is a monotonically non-decreasing function of $k$
	- Limit of $w(k,t)$ as $k$ becomes large is finite because address space is finite
- Implies that there is a large range of $k$ where working set is relatively unchanged
- As working set varies slowly over time, possible to prepage

- Define working set as set of pages used during the last 100ms of execution (rather than the last $k$ memory references as this leads to expensive calculations)
	- **Current Virtual Time:** CPU time that process has actually used
- **More formal definition of working set:** Set of pages used during last $\tau$ seconds of virtual time 

## Working Set Page Replacement Algorithm
- **Idea:** Find page not in working set and evict it
![[Working Set Algo.png|500]]
- **Con:** Entire page table has to be scanned to find suitable candidate

## WSClock Page Replacement Algorithm
![[WSClock Page Replacement Algo.png|600]]
- At each page fault, the page pointed to by hand is examined
	- If $R = 1$, time of last use updated to current virtual time and $R$ set to 0. Hand is advanced
	- If $R = 0$, 
		- If $age \geq \tau$ and $M=0$, current page not in WS and valid copy exists in SSD. Page is claimed and new page is written onto circular list
		- If $M=1$ (dirty page), disk rewrite operation is scheduled, but hand is advanced first and algorithm continues with next page
			- In theory, all pages might be scheduled for rewrite. To reduce non-volatile storage traffic, limit can be set for max number of pages to write back

- If hand comes all the way back to original position, there can be 2 scenarios
	- At least 1 write scheduled
		- Hand just keeps moving and looks for a clean page. As there are scheduled writes, hand will eventually come across a clean page and first clean page is evicted
	- No writes scheduled
		- All pages in working set. Simplest way is to claim clean page and use. If no clean page, current page chosen as sacrifice