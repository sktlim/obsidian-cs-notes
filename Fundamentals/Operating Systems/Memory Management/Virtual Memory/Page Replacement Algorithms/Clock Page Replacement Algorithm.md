> **Clock Page Replacement Algorithm:** Keep all page frames on a **circular list**, with pointer to oldest page
## How it works
- When page fault occurs, page referred to by pointer inspected
	- If referenced bit *R* is set, its cleared and pointer moves to next element
	- If referenced bit *R* is not set, page is evicted and new page is inserted. Pointer moves to next element
- Process repeated until second case is found