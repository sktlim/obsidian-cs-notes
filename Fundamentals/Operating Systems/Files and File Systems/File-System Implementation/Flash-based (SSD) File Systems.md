For full hardware details, see [[Solid-State Drives (SSDs)]]
## Quick Overview: SSD Internals 
![[Flash Internals.png]]
- Components
	- **Flash Packages (FP)**: Contains multiple dies, with each die containing a number of planes, collections of flash blocks containing flash pages
	- **Flash Translation Layer (FTL):** In charge of key responsibilities, including
		- **Logical-to-Physical Address Mapping**
		- **Wear-leveling**
		- **Garbage Collection**

- Reads are much faster than writes
	- This is because to write a flash page, a flash block has to be deleted
	- **Flash Page:** unit of IO (usually 4KB)
	- **Flash Block:** unit of erase (64-256 units of IO, usually a few MB)

- To write a flash page, SSD must first erase a flash block
- SSD cannot write out of order i.e. it must write onto page 0, 1, 2 of a block and cannot do something like 2, 0, 1
- SSD cannot update a flash page by itself in-place; it must store and erase the entire flash block, update the flash page, then write the whole block in again (process known as **write amplification**)
- Rewriting on the same flash cell wears out the flash cell
	- **Program/Erase (PE) Cycle**: Consists of erasing cell and writing new content 
	- Typical flash memory cells have a few thousand to few hundred thousand cycles before they expire
	- Device component in charge of handling wear-leveling is known as the **Flash Translation Layer (FTL)** 

- Small random writes cause issues for **garbage collection**, because they fragment blocks, requiring the consolidation of valid data and frequent erasures, which increases write amplification and degrades performance.

- When file system frees page, it **does not** need to explicitly tell the SSD that it freed the page
	- file system may use `TRIM` command to tell SSD pages are free (SSDs without `TRIM` still work but less efficiently)

## Which filesystem works best for SSDs?
- To summarize previous portion, SSDs have good read performance, poor random write performance, with frequent writes to same flash cells degrading lifetime
- **LFS** good match because of sequential writes, but [[Log-structured File Systems (LFS)#Wandering Tree Problem]] is still an issue
	- F2FS solved this through constant address mapping (See [[Log-structured File Systems (LFS)#Solutions]])
