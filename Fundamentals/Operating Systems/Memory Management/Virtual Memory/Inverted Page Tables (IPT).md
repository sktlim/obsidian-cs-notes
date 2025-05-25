> **Inverted Page Tables (IPT):** 1 entry per page frame in physical memory, rather than 1 entry per page in virtual address space

![[Inverted Page Tables.png|600]]

## Pros and Cons
### Pros
- Saves a lot of space

### Cons
- Virtual-to-physical translation becomes much harder
	- If process $n$ looks for page $p$, it has to look through the whole IPT for pairing ($n, p$) 
	- Search done on every memory reference, not just page faults
	- **Solution:** Use TLB to hold most heavily used pages

	- However, TLB miss still poses issues 
	- **Solution:** Hash virtual page + PID for a unique index to get corresponding physical frame
		- Hash collisions resolved using chaining 

## Hashing in IPT
### How Hashing Works

1. **Hash Function**:
    - A hash function takes the **virtual page number (VPN)** and **process ID (PID)** as input and computes a unique index (hash key).
    - This index determines where to look for the matching entry in the inverted page table.
    Example of a hash function: $$Hash\space Key = (VPN + PID)\space mod \space \text{table size}$$
2. **Lookup**:
    
    - The calculated hash key points to the **candidate entry** in the table.
    - The system checks the entry to see if the VPN and PID match.
    - If there’s no match, it may look at other locations (depending on the collision resolution strategy).
3. **Collision Handling**:
    - **Collisions** occur when two different VPN+PID pairs produce the same hash key.
    - To handle this, common techniques include:
        - **Chaining**: Use a linked list or bucket to store multiple entries at the same hash key.
        - **Open Addressing**: Probe other table slots using a systematic approach (e.g., linear probing or quadratic probing).
### Steps in Virtual-to-Physical Translation with Hashing
1. The CPU generates a **virtual address** with a VPN and PID.
2. The hash function computes the hash key for the VPN+PID pair.
3. The system uses the hash key to look up the inverted page table.
4. If the entry matches the VPN and PID:
    - The corresponding physical frame number is retrieved.
5. If there’s no match:
    - The system resolves collisions or declares a **page fault**.
## Applications
- Used in systems with large virtual address space and small physical address space