**Address space:** Set of addresses that process can use to address memory

Each process has its own address space, independent of other processes

### Simple and previously common way
#### Base and limit registers
- Simple version of **dynamic relocation**
- Map each process' address space onto different part of physical memory
- Equip each CPU with 2 special hardware registers, **base** and **limit**
	- When these 2 are used, programs loaded into consecutive memory regions wherever there is room and without relocation during loading
	- Base register loaded with physical address where program begins in memory and limit register with length of program
	- Every time process address memory, CPU automatically adds this offset. If address is higher than limit, fault is generated and access terminated
- **Disadvantage**
	- Need to perform addition and comparison on every memory reference
	- Additions are slow due to carry-propagation time unless special addition circuits used

#### Swapping
![[Memory Swapping.png]]
- Physical memory typically not large enough to hold all processes 
- Processes can be **swapped**, meaning they are ran for a while, then put back on non-volatile storage
- Can cause multiple holes to be created in memory
- **Memory compaction:** Move all processes down as far as possible to get big chunk of unused memory
	- Usually not done because it takes a lot of CPU time

### Managing Free Memory
- If memory dynamically assigned, OS needs to manage it
![[Memory Management.png]]
#### Bitmaps
- Memory divided into allocation units as small as a few words and as large as several kilobytes
- Corresponding to each allocation unit is a bit: 0 for free, 1 for occupied
- Size of allocation unit is design issue
	- Smaller the allocation, larger the bitmap. Even with allocation unit as small as 4 bytes, 32 bits of memory will require only 1 bit of map. Memory of $32n$ bits will require $n$ bits bitmap
	- If allocation unit too large, bitmap smaller, but memory might be wasted in last unit of process 
- **Advantage**
	- Easy way to keep track of memory words in fixed amount of memory because size of bitmap depends only on size of memory and size of allocation unit 
- **Disadvantage**
	- When $k$-unit process is being brought into memory, memory manager needs to search bitmap for $k$ consecutive 0 bits in bitmap, which is slow as the run may **straddle word boundaries in the map**
		- One word is like one line in 3-6b; the bitmap isn't continuous so I need to check line by line, something like retrieving multiple cache lines

#### Linked Lists
- Memory is being tracked using a linked list
	- Each entry is either a process (P) or a hole (H) and contains
		- address it starts at
		- length of entry
		- next entry pointer
- More convenient to have list in diagram as a doubly linked list so that termination of process X allows easy checks to see if merge (merging neighbor holes) is possible 

##### Linked Lists Memory Allocation Algorithms
- When processes and holes are kept on list sorted by address, several algorithms can be used to allocate memory for a created process
1) **First Fit**
	1) Scans list of segments until it finds hole big enough
2) **Next Fit**
	1) Works same as first fit, but keeps track of where it is when it finds hole
	2) Next time its called to find hole, it starts searching list from where it left off
	3) Slight worse performance than first first
3) **Best Fit**
	1) Searches entire list and takes smallest adequate hole
	2) Slower than first fit (trivial) 
	3) Results in more wasted memory because it tends to fill memory with small, useless holes
4) **Worst fit**
	1) Take largest hole possible
	2) Bad idea
5) **Quick fit**
	1) Maintains separate list of common sizes
		1) e.g. table of n entries, first entry is a pointer to head of list of 4KB nodes, second 8KB holes... 
	2) Finding hole extremely fast, but checking merge is expensive
		1) If merging not done, memory fragments into large number of small holes