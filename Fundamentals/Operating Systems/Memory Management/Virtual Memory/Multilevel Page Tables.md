**Page table walk:** Going through hierarchy of page tables

## How it works
![[Multilevel page table.png|500]]
- Pages are 4KB as offsets are 12 bits
- Total of $2^{20}$ pages
- Each entry in **top-level page table** represents 4M ($2^{22}$ bytes) because 32-bit address space is chopped into chunks of 4096 bytes (4KB / $2^{12}$) 

**Page Table Walk**:
- The virtual address is split into parts corresponding to each level.
- The CPU uses the **Page Table Base Register (PTBR)** to locate the top-level table.
- At each level, it indexes the table using the relevant part of the virtual address.
- The process continues until the final page frame is located.
## Structure (x86-64)
Multilevel page tables divide a virtual address into multiple parts corresponding to the hierarchy levels.  

For example, in a **4-level page table** (used in x86-64):
```
Virtual Address (64-bit):  
PML4   | PDPT   |   PD   |   PT   | Offset    
9 bits | 9 bits | 9 bits | 9 bits | 12 bits
```

- **Page Map Level 4 (PML4)**: Top-level table.
- **Page Directory Pointer Table (PDPT)**: Second-level table.
- **Page Directory (PD)**: Third-level table.
- **Page Table (PT)**: Fourth-level table, which points to physical frames.
- **Offset**: Specifies the byte within the final physical page.

Each level has 512 entries (9 bits = $2^9=512$).

**Canonical Addresses**:
- Only 48 bits are used, and the remaining bits (16 most significant bits) are required to follow a specific pattern to ensure compatibility:
    - Bits 48–63 must be a sign extension of bit 47. This ensures all virtual addresses are **sign-extended** to 64 bits.

Provides $2^{48} = 281,474,976,710,656 \text{ bytes} = 256 \text{ terabytes}$
