>How do we keep track of which disk blocks go with which file?

## Contiguous Allocation
![[Contiguous Allocation.png|500]]
- Store each file as contiguous run of disk blocks
- Each file begins at start of new block (so no half-blocks)
### Pros
- Simple to implement, only need keep track of starting block of file and length of file
- Read performance good because entire file can be read from disk in single operation
	- Only 1 seek needed to go to starting block, and the whole piece of data can be read in

### Cons
- Over time, disk becomes fragmented
- Disk usually not compacted on the spot because its an expensive operation, resulting in many holes similar to diagram in 4-12-b
	- Reuse of space can be done, but need to keep track of where the holes are
	- New file being created will also need to know its final size, which is impractical for many things

## Linked List Allocation
![[Linked List Allocation.png|400]]
- First part of each block used as pointer to next block, rest of block used for storing data
### Pros
- Every disk block can be used and no space is lost to disk fragmentation (except for internal fragmentation in last block on disk)
### Cons
- Random Access really slow
- Amount of data storage in a block no longer power of 2 because pointer takes up a few bytes, which might slow down programs assuming power of 2 storage

## Linked List Allocation using Table in Memory
![[Linked List Allocation using Table in Memory.png|500]]
- File A uses 4,7,2,10,12, while B uses 6,3,11,14
- **File-Allocation Table (FAT)** in memory keeps track of where the next block is for that particular file, so we can start with the first block and follow the chain until its terminated by a special marker (invalid block number)

- Original MS-DOS file system and still fully supported by all versions of Windows
- Still commonly used on SD cards for cameras etc 
### Pros
- Sufficient for directory entry to keep the starting block number to locate all its blocks
### Cons
- Entire table must be in memory all the time which does not scale well to large disks
	- Example
		- 1TB disk, 1KB block size, table needs 1 billion entries
		- Each entry minimum 3 bytes (for speed in lookup, they should be 4 bytes)
		- Table takes up 2.4GB - 3 GB of main memory all the time

## I-Nodes
![[I-node Example.png|400]]
- Associate each file with an inode which lists attributes and disk addresses of the file's blocks
### Pros
- Inode only needs to be in memory when file is opened
- Only requires an array in memory whose size is proportional to number of opened files, instead of being proportional to total number of entries like in previous example
## Cons
- If each inode has room only for a fixed number of addresses, what happens when a file grows beyond its limits?
	- One solution is to reserve last disk address not for a data block, but for disk block containing additional addresses (indirect block pointers inodes)
	- Using indirections, the issue of big files becomes more manageable

See also [[inode]]