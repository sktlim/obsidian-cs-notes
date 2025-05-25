>Keep log of what file system is going to do before it does it, so it can recover even when it crashes

## Problem
- When the file system crashes during some operation, OS doesn't really have a good way of recovery
- Example: File removal operation
	- Steps needed:
		1) File removed from directory
		2) Inode freed and placed into pool of free inodes
		3) Disk block freed and placed into pool of free disk blocks
	- If crash occurs anywhere and any of these 3 steps not taken, results are not good

## Solution: Journaling File System
- Write log entry detailing steps taken, and write this log entry to disk (for good measure, read it back to ensure that its correct)
- Begin operations, and 
	- if operations successful, log entry erased
	- If system crashes, file system can check log upon recovery to see if any operations were pending, and subsequently rerun them until file correctly removed

- Logged operations must be **idempotent**
	- Example of non-idempotent operations: Adding newly freed blocks to end of free list not idempotent as blocks might already be there (modifying this to "search list of free blocks and add block if not there" is idempotent)
- Under these conditions, crash recovery can be made secure

- File systems can also introduce **atomic transactions**
	- Actions grouped together into a transaction, and success becomes all-or-none