> Where does page go on non-volatile storage when its swapped out?

## Initial Approach
- Simplest solution is to have special swap partition on disk, or even on a separate storage device from the file system (UNIX systems work like this)
- This partition does not have file system on it (eliminates overhead of converting offsets in files to block addresses), and block numbers are relative to start of partition throughout

- When system booted, swap partition empty and represented in memory as a single entry giving origin and size
- When first process started, a chunk of the partition area the size of the first process is reserved and remaining area reduced by that amount
- As new processes started, they are assigned chunks of the swap partition equal in size to their core images
- As they finish, storage space is freed. Swap partition is managed as list of free chunks

- Each process saves info about where its non-volatile storage swap area is in its process table
- Before a process starts, its swap area must be initialized
	- Entire process image can be copied into swap area, so that it can be brought in as needed
	- Entire process image can also be loaded in memory so that it can be paged out as needed

## Issues with approach
- However, since processes can increase in size after starting, it may be better to reserve separate swap areas for text, data and stack and allow these areas to consist of more than 1 chunk on non-volatile storage
- Other extreme is to allocate nothing in advance and only allocate space on non-volatile storage when page is swapped out and deallocate it when its swapped back in. Disk address will then be needed to be stored in memory so that page can be tracked on non-volatile storage
- ![[Allocating space vs not in swapping pages.png|550]]

## Final Notes
- Having this swap partition on disk not always possible
- In this case, 1 or more large, preallocated files within the normal file system can be used instead (This approach is used by Windows)
- **Optimization:** Since program text of each process came from an executable file somewhere in the system, the executable file can be used as the swap area. And since program text is usually read-only, when program pages have to be evicted from memory, they are just discarded and read in again from executable file when needed

