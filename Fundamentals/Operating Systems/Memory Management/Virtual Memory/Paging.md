> When virtual memory is used, virtual addresses don't go directly to memory bus, but instead go to the **Memory Management Unit (MMU)**, which maps virtual addresses to physical addresses
![[MMU Example.png]]

## Definitions and Terminology
- **Page:** segment in virtual address space
- **Page Frame:** segment in physical memory
- **Page table:** Contains mapping of pages to page frames
## How it works
- Given a computer that can generate 16-bit addresses (0-64K), with 32KB of physical memory, we cannot load 64KB programs in its entirety into physical memory to run
- These pieces can be brought in as needed from non-volatile storage

![[Page table example.png|500]]
- Virtual address space contains fixed-sized units **pages**, with corresponding units in physical memory being called **page frames**
	- Pages and page frames have the same size
- With system specs as above, assuming we have page size of 4KB, we will have:
	- 16 virtual pages ($\frac{64}{4}=16$)
	- 8 page frames ($\frac{32}{4}=8$)
- Transfers between non-volatile storage and RAM are always in whole pages
- Many processors support multiple page sizes that can be mixed and matched as OS sees fit
	- x86-64 Architecture supports 4KB, 2MB, 1GB pages, so user apps can use 4KB and kernel can use 1GB for example
- If processor executes instruction `MOV REG, 0`, it will send instruction to MMU, which upon seeing the instruction accessing virtual address 0, will then map it to physical address 8192 and outputs address 8192 on the bus
- A **present/absent** bit keeps track of which pages are physically present in memory

### Page Fault
- If program references an unmapped address (e.g. `MOV REG, 327680`), MMU sees unmapped address and causes CPU to trap to OS, causing **page fault**
- OS picks a little used page frame and writes its content back to disk
- OS then fetches the page that was just referenced into the page frame just freed, changes map, and restarts trapped instruction

### MMU Internals
![[MMU Internals.png|400]]
- Incoming 16-bit address (8196) split into 
	- 4-bit page number ($2^4=16$ pages )
	- 12-bit offset ($2^{12}=4096$ bytes addressable within a page)
- Page number ($0010_{2} = 2_{10}$) indexes into the page table which gives the value of the corresponding page frame in physical memory (along with present/absent bit)
	- If present/absent bit = 0, OS traps 
	- If present/absent bit = 1, page frame number (110 in example above) is copied into first 3 bits of outgoing physical address, and remaining 12 bits are used for offset

## Structure of Page Table Entry
![[Page Table Entry.png]]
- 64-bits is common size nowadays
- **Page Frame Number:** Segment of corresponding physical address
	- If page size is 4KB ($2^{12}$ bits), we only need MSB 52 bits and can use the rest of the 12 bits for metadata
	- Most 64-bit CPUs use 48-bit addresses by design, so 36-bits is enough for page frame number
- **Present/Absent bit**
- **Protection bit(s)**
	- 1 bit: 1 for read only, 0 for R/W
	- 3 bits: RWX
- **Supervisor bit**: Indicates whether page is only accessible to privileged code
	- Any attempts by user program to access will result in a page fault
- **Modified (dirty) bit**: Set when page is written to
	- Useful if OS wants to reclaim page frame
		- If bit set (dirty), page frame must be written back to non-volatile storage
		- Else (clean), can just be abandoned since copy on storage is still valid
- **Referenced bit:** Set whenever page is referenced for R/W
	- Value used to choose page to evict when page fault occurs
- **Caching bit:** Allows caching to be disabled for that page
	- Important for pages that map to device registers rather than memory

## Speeding up Paging
- Current issues
	1) Mapping from virtual address to physical address must be fast
		1) Page table lookup should not be bottleneck of an operation execution
	2) Even if virtual address space is big, page table cannot be too big
		1) For modern processors with 64 bits, assuming 4KB pages and 48-bits used for address, that's still 64 billion pages in virtual address space

To solve problem 1, we have [[Translation Lookaside Buffers (TLB)]]
To solve problem 2, we have [[Multilevel Page Tables]]