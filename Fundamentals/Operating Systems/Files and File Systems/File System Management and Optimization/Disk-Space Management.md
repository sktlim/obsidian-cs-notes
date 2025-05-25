Strategies for storing $n$ bytes file:
- $n$ consecutive bytes of disk space allocated
- File is split into a number of blocks

Splitting is preferred because its generally faster to move blocks around in memory as compared to move files around on disk

## Block Size
- If block size too small, we waste time. If block size too big, we waste space
- Block size of 4KB reasonable compromise for average users
- ![[Disk space utilization tradeoff.png]]
- Performance and space utilization are inherently in conflict
	- Small blocks bad for performance but good for disk-space utilization
- Above is in perspective of HDD. With flash storage, we incur memory waste not just for large disk blocks, but also for smaller ones that do not fill a flash page

## Tracking Free Blocks
![[Tracking free blocks in storage.png]]
- **Storing free list on linked list of disk blocks**
	- With 1Kb block and 32-bit block identifier number, 1 block can store 255 other block numbers (with last block pointing to next disk block containing identifiers)
	- For 1TB disk (~1 billion disk blocks), around 4 million blocks required to store all free blocks
	- Only 1 block of pointers need to be kept in memory. When it runs out, new block of pointers read from disk, current block written to disk
		- However, this leads to inefficient I/O when there are short-lived temporary files when block of pointers in main memory is almost full
			- ![[free list disk io inefficiency.png]]
			- If 3 block file is freed, block in main memory overflows and current block has to be written to disk (4th column in (b)). If 3 block file is written again, we go back to (a), resulting in unnecessary disk ops. 
			- Mitigation: write only half of block to disk as per (c). Temporary files can be handled better
- **Bitmap**
	- Disk with $n$ blocks require bitmap with $n$ bits
	- 1TB disk with 1 billion blocks requires 130,000 1KB blocks to store
	- Requires less space than free list
- If free blocks come contiguously, free list system can be modified to keep track of this
	- Count can be associated with each block giving number of free blocks
	- Empty disk can be represented with address of first free block followed by count of blocks
	- If disk becomes severely fragmented, this can result in inefficiencies

## Disk Quota
- For preventing users from hogging disk space in multiuser OS, sysadmin assigns each user a max allotment of files and blocks
- ![[disk quota user.png]]
- When user opens file, attributes and disk addresses located and placed into open file table in main memory
- Quota table keeps track of record for all users with currently open file (even if file opened by someone else)
- Soft limit may be exceeded, hard limit not (OS throws error)
- Number of files also checked to prevent i-nodes from being hogged
- When user attempts to login, system examines file quota to see if user has exceeded soft limit (either number of files or number of disk blocks) and if exceeded, reduces number of file warnings left by 1. When this hits 0, user not permitted to login













