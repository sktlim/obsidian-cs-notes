> Aging of pages works best if there are free page frames to claim. To ensure there are enough free page frames to claim, a **paging daemon** runs periodically to evict pages. An adequate cleaning policy must therefore be chosen for the paging daemon

## Possible Implementation
- 2-pointer circular list
	- 1st pointer controlled by daemon; 
		- If pointer points to dirty page, page is written to non-volatile storage and pointer is advanced
		- If pointer points to clean page, pointer is just advanced
	- 2nd pointer used for normal page replacement like in normal clock algorithm (not special to this particular policy)
		- Probability of 2nd pointer hitting clean page higher since 1st pointer doing most of the work already