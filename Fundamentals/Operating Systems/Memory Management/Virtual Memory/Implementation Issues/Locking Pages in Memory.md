> Evicting a page involved in an ongoing I/O operation can corrupt data due to partial overwrites caused by DMA transfers.

## Problem
- When a process performs an I/O operation (e.g., reading into a buffer in its address space) and is suspended to allow another process to run, the I/O buffer page may be evicted from memory by the global paging algorithm. 
- If the I/O device is performing a **DMA (Direct Memory Access)** transfer to this page, evicting it can lead to corrupted data, as parts of the transfer may overwrite unrelated memory.
## Solution
1. **Lock (Pin) Pages in Memory**: Prevent pages involved in I/O operations from being evicted by marking them as locked in memory.
	- System calls `mlock()` and `mlockall()` in Linux
2. **Kernel Buffers for I/O**: Perform all I/O operations in kernel-managed buffers and copy the data to user-space pages after the I/O is complete.
    - **Trade-off**: This avoids corruption but adds an extra memory copy, reducing performance.