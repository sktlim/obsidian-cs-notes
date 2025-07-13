## File System Performance
Since reading from hard disk is much slower than from memory or from flash, various optimizations are introduced to make this faster. 

## Caching
**Block Cache/ Buffer Cache**: Collection of blocks that belong on disk but are kept on memory for performance reasons

Common algorithm to manage cache is to check all read requests to see if memory block is needed in cache. To quickly look up and determine if memory block is present, usual way is to hash device and disk address to use as key. Hash collisions managed using **closed addressing** [[Hashing]]

![[Buffer cache data struct.png|500]]

The difference between paging and file system caching is that, in caching, references to cached blocks occur less frequently. This makes it feasible to maintain an exact LRU (Least Recently Used) order using linked lists, unlike in paging where the high frequency of memory accesses makes such tracking too costly.

Another key difference: Files are in the page cache and disk blocks are in the buffer cache. 
- Some files in the page cache are typically on disk also, so data is now in both caches. 
- Some OSes therefore integrate buffer cache with page cache. 
- This is especially attractive when memory-mapped files are supported. 
	- If file is mapped onto memory, some of its pages may be in memory because they were demand paged in, and aren't much different from file blocks in buffer cache and therefore can be treated the same way

Referencing above figure, exact LRU order is maintained by managing the memory block's position on the bidirectional list.

### Cons
However, **pure LRU is undesirable**. 
- If a critical block such as an inode block is read into cache and modified, but not rewritten to disk, a crash will leave file system in an inconsistent state. If inode block placed on rear of LRU, it might take some time before it is rewritten.
- Inode blocks are rarely referenced 2 times within a short time interval

### Solution: Modified LRU Scheme
Main considerations:
1) Is block likely needed again soon?
2) Is block essential to consistency of file system?

- Blocks that will **not** be needed again soon go to the front of the LRU list so that their buffers will be reused quickly
- Blocks that might be needed again soon go on end of the list so that they stay around for a longer time
- If block is essential to file system consistency, and has been modified, it should be written to disk immediately regardless of where it is on LRU
	- Reduces risk of file system inconsistency

- It is undesirable to keep data blocks in cache for too long before writing them out
	- UNIX: `sync` system call forces all modified blocks to be written onto disk immediately. When system is started, update program is started in background to sit in endless loop issuing `sync` calls every 30 seconds
	- Windows: `FlushFileBuffers` are the equivalent system call to `sync`. In the past before this, it wrote all modified blocks immediately back to disk as soon as it was in the cache. This is known as **write-through caches**. They require more disk IO than non-writethrough caches.
- Side track
	- Difference was because UNIX written in an environment in which all disks were hard disks and not removable, while Windows was inherited from MS-DOS which started out with floppy disks
- 

## Block Read Ahead
Another way to improve perceived file system performance is to try to get blocks into cache before they are needed to increase hit rate
- Many files are read sequentially, so loading the $k+1$ file into the cache before its read should improve performance
- This strategy fails for randomly accessed files
	- Hurts more by using disk bandwidth to read useless blocks and potentially removing useful blocks from cache
	- File system can track access patterns to each open file to see if this is worth doing
	- e.g. Bit associated with each file can track whether file is in sequential access mode or random access mode

## Reducing Disk-Arm Motion
Put blocks that are likely to be accessed in sequence close to each other (preferably in same cylinder)
- When writing output files, if free blocks are in a bitmap, it is relatively easy to choose free blocks close to each other. 
- With a free list, it is harder but some block clustering can still be done
	- Idea is to keep track of disk storage not in blocks, but in groups of consecutive blocks
	- If all sectors consist of 512 bytes, system can use 1KB blocks but allocate storage in 2 blocks
- Variation is to take account of rotational positioning by trying to allocate blocks on same cylinder

## Another Performance Bottleneck
For inode systems, reading even a short file requires 2 disk accesses, 1 for inode and 1 for block.
![[Inode on disk.png]]
Many file systems have inode placement like the one in (a).
- Inode at start of disk, so average distance between inode and blocks will be half the number of cylinders and require long seeks

Easy performance improvement: put inode in middle of disk
- This reduces average seek between inode and first block by factor of 2

Another idea: divide disk into cylinder groups, each with its own inodes, blocks, free list (shown in (b))
- When creating new file, any inode can be chosen, but attempt is made to find block in same cylinder group as inode
- If none available, block in nearby cylinder group is used

## Flashcards

