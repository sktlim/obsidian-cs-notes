>**Translation Lookaside Buffers (TLB)**: high-speed cache in the CPU that stores recent virtual-to-physical address translations to speed up memory access in virtual memory systems. Also known as **associative memory**
## Properties
- Usually inside MMU
- Consist small number of entries (less than 256)
## Intuition of TLB
- For small instruction sets, fetching from memory introduces unnecessary overhead as the page table has to be accessed before one can fetch the relevant memory information
- Observed that most computer programs tend to make a large number of references to a small amount of pages, so this phenomena is used to solve unnecessary access of page table

- TLB misses are a lot more frequent than page fault, because there is plenty of physical address space, while TLB typically only holds 64 entries
## How TLB works
![[TLB example.png]]
1) MMU receives virtual address for translation
2) Hardware check if virtual page number in TLB by comparing to all entries simultaneously
	1) If valid match found and access doesn't violate protection bits, page frame taken directly from TLB
	2) If protection bit violated, protection fault generated
	3) If no match, MMU detects miss and does ordinary page table lookup
		1) MMU evicts one of entries from TLB and replaces it with page that was just looked up
		2) Modified bit of evicted page is copied back into page table entry in memory

- If OS wants to change bits in TLB's page table entry, it will make the modification in memory, then flush the old TLB entry with the outdated permission bits

#### Software TLB Management
- Assumed that TLB management and handling TLB faults done by MMU hardware and traps to OS only occurs when page not in memory
- On some RISC machines, support for TLB directly provided in software
	- TLB entries are explicitly loaded by OS
	- When TLB miss occurs, it just generates a TLB fault and hands control over to OS
- When TLB moderately large to reduce miss rate, software TLB management is acceptably efficient
- Offers benefit of simplifying MMU which frees up area on chip for caches and other things 

## Different Misses
- **Soft Miss:** Page referenced not in TLB, but in memory
	- TLB just needs to be updated and only 10-20 machine instructions needed (nanosecond completion time)
- **Hard Miss:** Page referenced not in TLB and not in memory
	- Access to disk or SSD required, taking up to milliseconds

- Some misses can be harder/softer than others
	- Page walk does not find entry in process' page table and incurs a page fault
		- Page might be in memory, but not that process' page table e.g. brought in by another process (**Minor page fault**). Just need to map page appropriately in page tables
		- Page needs to be brought in from non-volatile storage (**Major Page Fault**)
		- Program accessed invalid address and no mapping needs to be added (**Segmentation Fault**)