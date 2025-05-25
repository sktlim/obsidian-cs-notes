OS has to do paging stuff during:
1) Process creation time
	- OS determines how large program and data will be and creates a page table for process
	- Space has to be allocated for page table and page table has to be initialized
	- Space has to be allocated on swap area in non-volatile storage for page when its swapped out
	- Swap area has to be initialized with program text and data so that when process get faults, there is another program ready to be swapped in
	- Info about page table and swap area on non-volatile storage must be recorded in process table
1) Process execution time
	- MMU has to be reset for new process
	- Unless entries for TLB are explicitly tagged with process IDs (PIDs) (**tagged TLBs**), TLB has to be flushed
	- New process page table has to be current , i.e. page table/ pointer to page table has to be loaded onto hardware register
	- Some pages can be initially loaded to memory to reduce page faults. Page pointed to by program counter (PC) must be loaded
1) Page fault time
	- OS must read hardware registers to determine which page caused the fault
	- Missing page that caused fault must be retrieved from non-volatile storage
	- Page in page table must be evicted
	- New page must be loaded into page frame
	- PC must be reset to instruction that caused the fault
1) Process termination time
	1) OS must release page table, pages, and non-volatile storage space that pages occupy on disk/SSD
	2) If some pages shared, pages can only be released when last process using them has terminated

## Page Fault Handling
1) Hardware traps to kernel, saving PC on stack. Info on state of current instruction saved onto special registers
2) An **interrupt service routine (ISR)** is started to save registers and other volatile information. It then calls the page fault handler
3) OS uncovers which virtual address caused the fault, usually through accessing hardware registers. If not, OS gets PC, replays the instruction, then finds it
4) System checks if virtual address is valid, along with whether protection bits are correct. If not, process is sent a signal or killed. Else, OS checks if page frame is free. If no pages free, page replacement algorithm run to evict page
5) If page frame dirty, page scheduled for transfer to non-volatile storage, and context switch takes place, suspending faulting process and letting another run until disk/SSD transfer completes. Frame is marked as busy so that other processes won't try to access it
6) As soon as page frame is clean, OS looks up disk address where needed page is and transfer it into the page frame. While page is being loaded, process is suspended and another process is ran if available 
7) When disk/SSD interrupt indicates page has arrived, page tables updated to reflect its position, and frame is marked as being in the normal state
8) Faulting instruction backed up to before it went into that state along with PC
9) Faulting process is scheduled, and OS returns to state before ISR calls it
10) ISR reloads registers and other state info and returns to user space to continue execution

This process leads to a bunch of issues in implementation down the line, namely:
- [[Instruction Backup]]