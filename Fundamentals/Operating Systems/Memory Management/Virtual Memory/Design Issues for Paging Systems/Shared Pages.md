> Sharing pages reduces overhead, but not all pages are shareable. Data page sharing is one example 

- For separate I and D spaces, I-space can be shared between 2 processes while each process will then have its own D-space
- When I space is being shared among 2 processes say A and B, if scheduler decides to remove A from memory, B will thrash until instructions are brought back for B

- In UNIX, after `fork()`, parent and child are required to share both program text and data
	- Each process will have its own set of page tables pointing to the same pages
	- No copying of pages done at `fork()`
	- All data mapped into both processes as read only, until write made, upon which own copy of data is made (**copy-on-write**)
