Larger caches have <u>better hit rates</u>, <u>longer latency</u>

- For caching lines of main memory in the CPU cache, a new item will generally be entered on every cache miss. 
- The cache line to use is generally computed by using some of the high-order bits of the memory address referenced. 
	- For example, with 4096 cache lines of 64 bytes and 32 bit addresses, bits 6 through 17 might be used to specify the cache line, with bits 0 to 5 the byte within the cache line.

Caches form a memory hierarchy (L1 fastest, L3 largest)

**Data word**
> standard unit of data that a processor can handle, process, or transfer, e.g. memory addresses, instructions 
## L1 Cache
- Always inside the CPU and usually feeds decode instructions into the CPU's execution engine
- Most chips have second L1 cache for heavily used data words
- Usually split between **instruction cache** and **data cache**
- In market as of **4 Nov 2024** 
	- Intel i9-14900K: 80KB per core

## L2 Cache
- Holds several MB of recently used data words 
- Key difference between L1 and L2 lie in timing; Accessing L2 cache may introduce delay of 10-20 clock cycles
- Sometimes there are **architectures that share L2 cache**, which results in more complicated cache controller but the per-core L2 caches makes keeping the caches consistent more difficult
- In market as of **4 Nov 2024** 
	- Intel i9-14900K: 2MB per core

## L3 Cache
- Shared among all cores
- Serves as a shared pool for all cores, making it useful for data that may need to be accessed by multiple cores. 
- L3 can act as a buffer before the CPU must go to main memory, minimizing costly memory access delays.
- 30-50 cycles for access or more
- In market as of **4 Nov 2024** 
	- Intel i9-14900K: 36MB shared

### Summary Table

| Feature         | **L1 Cache**                    | **L2 Cache**                 | **L3 Cache**                      |
| --------------- | ------------------------------- | ---------------------------- | --------------------------------- |
| **Location**    | Closest to CPU core, per core   | Inside or close to each core | Shared across all cores           |
| **Size**        | Small (32 KB - 128 KB)          | Medium (256 KB - 2 MB)       | Large (4 MB - 64 MB)              |
| **Speed**       | Fastest (1-4 cycles)            | Intermediate (10-20 cycles)  | Slowest of caches (30-50+ cycles) |
| **Function**    | Holds most frequently used data | Stores data not in L1        | Shared pool for all cores         |
| **Sharing**     | Dedicated per core              | Sometimes shared per 2 cores | Shared across all cores           |
| **Power Usage** | Lowest                          | Moderate                     | Highest                           |

### Why cache lines instead of caching individual bytes?
Fetching data in cache lines allows the CPU to take advantage of spatial locality, reduces the latency associated with individual memory accesses, maximizes bandwidth usage, and minimizes cache misses. 