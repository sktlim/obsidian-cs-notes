> Giving each program its own address space, which is then further broken up into chunks called **pages** where each pages is a contiguous range of addresses
## Old Solution: Overlays
- Split program into little pieces called overlays
- When program starts, first instruction loaded into memory was overlay manager, which subsequently loaded overlay 0, and overlay 1 in memory space above 0 (if there was space in memory above overlay 0)
	- If no space, it will call overlay 1 to override overlay 0
- Time consuming and error prone

## Virtual Memory
- Each program has its own address space, broken into chunks known as **pages**
- Pages mapped onto physical memory, but not all pages have to be loaded at the same time to run program

### How it works
- When program references a part of address space that is on physical memory, hardware does mapping on the fly
- Else, if program references a part of address space that is **not** on physical memory, OS is alerted to load page into physical memory and re-execute the failed instruction

See [[Paging]]
