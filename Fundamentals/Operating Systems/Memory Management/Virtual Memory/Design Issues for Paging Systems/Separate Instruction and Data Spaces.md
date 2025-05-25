> Most computer have single address space holding program and data, but if this gets too small, one solution is to separate it into 2 address spaces that individually contain the instruction (I-space) and data spaces (D-space)

![[Separate address spaces.png|500]]
- Linker must know when separate spaces are used as data is relocated to virtual address 0 instead of starting after the program
- Both address spaces can be paged independently of each other
	- Each address space has its own page table, with its own mapping of pages to page frames

- Nowadays, they're used to divide L1 cache (according to textbook) and even used to partition TLBs