> **Not Recently Used (NRU) Page Replacement Algorithm**: Removes a random page of lowest priority
## How it works
- Page table entry contains *R* and *M* bit
	- *R* bit: Set if page is referenced (R/W)
	- *M* bit: Set if page is modified
- Bits are updated on every memory reference, so its done by hardware
- Once bit set, it remains set until OS resets it

- When process is started up, *R* and *M* set to 0 for all pages
- *R* bit is periodically cleared to distinguish pages that have been referenced recently versus those that haven't 
- When page fault occurs, OS divides pages into the following categories:

| **Class** | **Referenced (R)** | **Modified (M)** |
| --------- | ------------------ | ---------------- |
| Class 0   | 0                  | 0                |
| Class 1   | 0                  | 1                |
| Class 2   | 1                  | 0                |
| Class 3   | 1                  | 1                |
- **NRU Algorithm:** Removes page at random from lowest-numbered non-empty class
	- Implied that a modified non-referenced page (reference cleared by clock tick) is less important that a recently referenced non-modified page (heavily used page)