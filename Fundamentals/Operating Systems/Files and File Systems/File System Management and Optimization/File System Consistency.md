## File-system Consistency
- If system crashes before all modified blocks have been written, fs can be left in inconsistent state
- Most computers have utility program that checks fs consistency
	- UNIX has `fsck`, Windows has `sfc`
- Some fs (e.g. journaling fs) designed such that admins not required to run separate fs consistency checker after crash as they can handle most inconsistencies themselves
### Block Consistency Check
![[File system block consistency check.png|600]]
- **Main Idea:** Program builds 2 tables, each one containing counter for each block, with counter initially set to 0
	- 1st table counters keep track of how many times each block is present in file
	- 2nd table counters record how often each block present in free list (or bitmap of free blocks)
- Program reads all inodes using raw device (ignore file structure, returns all disk blocks starting at 0)
	- Program builds list of all block numbers used in corresponding file
	- Counter in first table is incremented as block number read
- Program then examines free list/ bitmap to find blocks that are not in use
	- Each occurrence increments relevant counter in second table

- If fs consistent, each block will have a 1 either in first or second table (as per **diagram a**). If there is a crash, table might look like **diagram b** (block 2 does not occur in either table and is reported as **missing block**)
	- Missing blocks waste space and reduces disk capacity. Solution is for fs checker to add them to free list
	- Duplicates might occur (for free list implementations, not bitmap) as per diagram c block 4. Solution is to just rebuild free list
- Same data block might be present in 2 or more files as per block 5 in diagram d
	- If either file removed, 5 will be put on free list, causing same block to be both in use and free at same time
	- If both files removed, block placed onto free list twice
	- **Solution**: fs checker allocate a free block, copy contents of block 5 into it, insert copy into one of the files
		- Content is unchanged (but most likely garbled) but at least structure is made consistent

## Checking Directories
- filesystem checker also checks directory system
	- uses a table of counters per file (rather than per block)
	- starts at root and recursively descends and inspects each directory
		- For every inode in every directory, it increments a counter for that file's usage count
		- Due to hard links, file may appear in 2 or more directories
		- Symbolic links don't count
- When checker is done, it will have list indexed by inode number telling how many directories contain each file
	- It compares this to link counts within inodes themselves
	- Counts start at 1 when file is created and incremented for each hard link made
	- In consistent system, numbers will be the same. However, 2 kinds of error can occur:
		- **Link count in inode too high**
			- Even if all files removed from directories, count still nonzero and inode will not be removed
			- Wastes space on disk with files that are not in any directory
			- Fixed by setting link count on inode to correct value
		- **Link count in inode too low**
			- Potentially catastrophic
			- If 2 directory entries linked to file but inode says 1, when either directory entry is removed inode count goes to 0. System marks this inode as unused and releases all of its blocks
			- Results in deadlink
			- Solution: Force link count to be the actual number of directory entries

- 