TARGET DECK: Interview Prep::Computer Archi

Q:  
What is the memory hierarchy in a modern CPU and why does it exist?  
A:  
Modern CPUs use a layered memory hierarchy—registers, L1/L2/L3 caches, main memory (RAM), and storage—to bridge the speed gap between the CPU and slower memory. Each layer trades off speed, size, and cost. Registers and L1 caches are fastest and smallest, while RAM and SSDs are slower but larger. The hierarchy ensures that most memory accesses are fast by keeping frequently used data close to the CPU.
<!--ID: 1748883231753-->


---

Q:  
What is a cache line and why is it important for performance?  
A:  
A cache line is the smallest unit of memory that can be loaded into the CPU cache—typically 64 bytes on modern CPUs. When a program accesses a byte in memory, the CPU loads the entire cache line containing that byte. Efficient programs access memory in patterns that align with cache lines (spatial locality), reducing cache misses and improving performance.
<!--ID: 1748883231756-->


---

Q:  
What is cache associativity and how does it affect performance?  
A:  
Cache associativity determines how memory blocks map to cache sets. For example, in a 4-way set-associative cache, each memory block can map to any of 4 cache lines in a set. Higher associativity reduces the chance of conflict misses (when multiple addresses compete for the same cache set), but increases access time and complexity. Poor associativity can lead to performance degradation due to frequent evictions.
<!--ID: 1748883231759-->


---

Q:  
What is the difference between temporal and spatial locality in memory access?  
A:  
Temporal locality refers to re-accessing the same memory location soon after it was last used (e.g., loop counters). Spatial locality refers to accessing nearby memory locations in a short time span (e.g., iterating through an array). Both forms improve cache effectiveness. Code optimized for cache usually loops through arrays linearly to leverage spatial locality and minimize cache misses.
<!--ID: 1748883231761-->


---

Q:  
What is a cache miss, and what are the three types?  
A:  
A cache miss occurs when the requested data is not found in the cache. The three types are:
1. **Compulsory (cold) miss**: the first access to a data item
2. **Capacity miss**: the cache is too small to hold all active data.
3. **Conflict miss**: multiple data items compete for the same cache line due to associativity limits.  
    Each miss results in longer access time as the data must be fetched from lower levels of memory.
    
<!--ID: 1748883231764-->


---

Q:  
How does false sharing impact performance?  
A:  
False sharing occurs when threads on different cores write to different variables that happen to share the same cache line. Even though they access different data, the entire line is invalidated in other cores' caches, causing excessive cache coherence traffic. This leads to performance degradation due to frequent cache line bouncing. Padding variables to separate cache lines avoids this problem.
<!--ID: 1748883231767-->


---

Q:  
Why is memory alignment important in low-latency systems?  
A:  
Memory alignment ensures that data structures are stored at addresses that match their natural alignment (e.g., 8-byte alignment for `double`). Misaligned accesses may require multiple memory operations or cause hardware exceptions. Proper alignment also helps fit data neatly into cache lines, improving performance and reducing the risk of false sharing.
<!--ID: 1748883231769-->


---

Q:  
What are hardware prefetchers and how do they help cache performance?  
A:  
Hardware prefetchers detect access patterns (like sequential array traversal) and fetch future cache lines before they are accessed. This reduces wait time for memory and hides latency. However, irregular access patterns may confuse prefetchers, leading to wasted bandwidth or cache pollution. For latency-critical systems, predictable access patterns help leverage prefetching effectively.
<!--ID: 1748883231772-->


---

Q:  
What is a TLB and how does it relate to caching?  
A:  
The Translation Lookaside Buffer (TLB) is a cache for virtual-to-physical address translations used by the CPU’s memory management unit (MMU). Every memory access involves address translation, and a TLB miss requires a page table walk, which is expensive. Keeping memory access localized to a small number of pages helps improve TLB hit rate, crucial in low-latency code.
<!--ID: 1748883231775-->


---

Q:  
How can software developers improve cache usage?  
A:  
Developers can:
- Use array-of-structures (AoS) or structure-of-arrays (SoA) depending on access pattern
- Access data sequentially to improve spatial locality
- Avoid large memory strides and random accesses
- Use `restrict` keyword in C/C++ to allow the compiler to optimize loads/stores
- Pad structures to avoid false sharing
- Use aligned allocations for SIMD and cache alignment
<!--ID: 1748883231777-->


---

Q:  
What is cache pollution and how can it be avoided?  
A:  
Cache pollution occurs when unnecessary data evicts useful data from the cache. For example, prefetching data that won’t be reused soon can displace frequently accessed items. It can be avoided by:
- Avoiding speculative prefetching of cold data
- Using software prefetching instructions with care
- Employing non-temporal memory operations for one-time reads/writes
<!--ID: 1748883231779-->


Q:  
What is the difference between inclusive and exclusive caches?  
A:  
In an **inclusive cache hierarchy**, each level (e.g., L2) contains all data from lower levels (e.g., L1). This simplifies coherence but increases duplication. In an **exclusive cache**, each level contains unique data, maximizing effective capacity but requiring more complex management. For latency-sensitive systems, inclusive caches provide faster invalidation but may waste space due to redundancy.
<!--ID: 1748883231782-->


---

Q:  
Why does accessing memory with large strides (e.g., every 64th element in an array) lead to poor cache performance?  
A:  
Large strides skip over many elements, preventing the use of **spatial locality**. Since only the accessed element in each cache line is used, the rest of the cache line is wasted. This leads to frequent cache line loads and evictions, increasing **cache miss rate** and **memory bandwidth pressure**.
<!--ID: 1748883231784-->


---

Q:  
How does the structure of a C++ struct/class affect cache utilization?  
A:  
The **layout of members** in a struct or class affects alignment, padding, and overall size. Poorly ordered fields may lead to inefficient use of cache lines and increased memory footprint. For optimal cache performance, group frequently accessed fields together and order by descending size to minimize padding. Tools like `sizeof()` and `alignof()` help in layout inspection.
<!--ID: 1748883231787-->


---

Q:  
What is cache line eviction and what policy is commonly used?  
A:  
Cache line eviction occurs when a new memory block needs to be loaded into a full cache set. The CPU uses a **replacement policy**, commonly **Least Recently Used (LRU)** or pseudo-LRU, to decide which existing cache line to evict. Eviction of hot data causes performance drops due to repeated memory fetches.
<!--ID: 1748883231789-->


---

Q:  
What is the role of software prefetching, and when is it appropriate?  
A:  
Software prefetching explicitly instructs the CPU to load data into cache using intrinsics like `__builtin_prefetch()` in GCC/Clang. It's useful when hardware prefetchers fail—e.g., irregular access patterns like linked lists. However, excessive prefetching can lead to **cache pollution** or wasted bandwidth.
<!--ID: 1748883231791-->


---

Q:  
Why does aligning data to cache line boundaries improve performance?  
A:  
Aligning data ensures that performance-critical structures do not straddle two cache lines, avoiding multiple loads for a single access. It also prevents unrelated variables from sharing the same line, mitigating **false sharing**. Alignment can be enforced using attributes like `alignas(64)` in C++11 or `__attribute__((aligned(64)))` in GCC.
<!--ID: 1748883231794-->


---

Q:  
How does SIMD affect cache usage and memory alignment?  
A:  
SIMD (Single Instruction Multiple Data) operates on wide registers (e.g., 128-bit, 256-bit, 512-bit). These require memory to be aligned to corresponding boundaries for efficient access. Misaligned loads can incur extra instructions or slow paths. Cache performance improves when SIMD loads/stores align with cache line boundaries and data is packed tightly.
<!--ID: 1748883231797-->


---

Q:  
What is the impact of cache coherence protocols (like MESI) on performance in multi-core systems?  
A:  
MESI (Modified, Exclusive, Shared, Invalid) maintains coherence among caches in different cores. When one core writes to a shared cache line, others must invalidate or update their copies, triggering **coherence traffic**. This adds latency, especially when threads frequently write to shared data. Minimizing shared writes and using thread-local storage helps reduce this impact.
<!--ID: 1748883231802-->


---

Q:  
How can you inspect and optimize for cache usage on Linux systems?  
A:  
Use tools like:
- `perf stat` for cache misses, cycles per instruction (CPI), and branches
- `valgrind --tool=cachegrind` for detailed simulation of cache behavior
- `likwid` or `toplev` for hardware counter monitoring. Optimization involves reducing memory footprint, improving locality, and aligning memory access with the cache hierarchy.
<!--ID: 1748883231806-->


---

Q:  
In what cases might disabling caches improve performance?  
A:  
Disabling caches is rarely beneficial. However, in certain **real-time embedded systems**, deterministic behavior is more important than raw speed. Caches introduce variability due to misses. Disabling caches (or using locked caches) can help maintain predictability at the cost of throughput—this is typically not used in trading systems, where both latency and throughput matter.
<!--ID: 1748883231809-->


Q:  
What happens when a CPU encounters a cache miss?  
A:  
When the CPU encounters a cache miss, it must fetch the requested data from the next lower level in the memory hierarchy. If the data is not in L1, it checks L2, then L3, and finally main memory. Each step introduces more latency, with main memory access taking hundreds of cycles. During this time, the CPU may stall or switch to executing other instructions if out-of-order execution is possible, but overall, a miss significantly degrades performance.
<!--ID: 1748883231812-->


---

Q:  
Why is L1 cache typically smaller but faster than L2 and L3?  
A:  
L1 cache is kept small to minimize latency, often just 32KB per core for instructions and data. Smaller caches can be accessed more quickly due to simpler hardware circuitry and shorter wiring distances. L2 and L3 caches are larger and slower because they store more data and require more complex lookup logic, but their larger size reduces the frequency of cache misses.
<!--ID: 1748883231814-->


---

Q:  
How does the cache replacement policy affect performance?  
A:  
The cache replacement policy determines which cache line to evict when loading new data into a full cache set. If the policy frequently evicts lines that are soon reused, it leads to more cache misses and degraded performance. A well-chosen policy like Least Recently Used (LRU) tries to retain recently accessed lines, which often results in better temporal locality and fewer misses, but imperfect heuristics or pseudo-LRU may still evict useful data under some workloads.
<!--ID: 1748883231817-->


---

Q:  
Why might a structure of arrays (SoA) layout improve cache performance compared to array of structures (AoS)?  
A:  
Structure of arrays layout stores each field of a structure in a separate contiguous block of memory. This layout improves spatial locality when accessing only a subset of the fields across many elements, allowing the CPU to load only relevant cache lines. In contrast, array of structures may load unnecessary data into the cache, wasting bandwidth and polluting the cache when only one field is used frequently.
<!--ID: 1748883231819-->


---

Q:  
How do direct-mapped and set-associative caches differ in terms of performance and complexity?  
A:  
A direct-mapped cache allows each memory address to map to exactly one cache line, which makes lookup fast but increases the chance of conflict misses. Set-associative caches allow a memory block to be placed in one of several lines in a cache set, reducing conflicts at the cost of more complex indexing and slightly longer lookup times. Fully associative caches allow placement anywhere but are impractical for large caches due to hardware complexity.
<!--ID: 1748883231822-->


---

Q:  
What is a cold cache and how does it affect system startup latency?  
A:  
A cold cache refers to the state when the cache is empty or contains irrelevant data, such as immediately after system startup or process launch. As the program begins executing, nearly every memory access results in a cache miss, forcing data to be loaded from slower memory. This leads to poor performance initially. Over time, as the cache is populated with relevant data, performance improves due to fewer misses.
<!--ID: 1748883231824-->


---

Q:  
Why is memory access order critical in high-performance loops?  
A:  
The order of memory accesses determines whether spatial and temporal locality are exploited effectively. Sequential access patterns allow the CPU’s prefetcher to bring in data ahead of time and reduce the chance of cache misses. Random or strided access patterns can cause the prefetcher to fail, increase TLB misses, and waste cache bandwidth, leading to degraded performance especially in tight loops.
<!--ID: 1748883231826-->


---

Q:  
How does hardware cache coherence work in multicore systems?  
A:  
Hardware cache coherence uses protocols like MESI to ensure that all cores see a consistent view of memory. When one core modifies a cache line, it sends invalidation messages to other cores holding that line in their cache. This ensures correctness but can introduce delays due to synchronization traffic. In low-latency systems, avoiding shared writable data and using thread-local storage helps reduce these coherence penalties.
<!--ID: 1748883231829-->


---

Q:  
What is write-back caching and how does it differ from write-through?  
A:  
In write-back caching, modifications to memory are made only in the cache initially, and the updated data is written to main memory later, typically when the line is evicted. This improves performance by reducing write frequency to slow memory. In write-through caching, every write to the cache also updates main memory immediately, which ensures consistency but increases latency and memory traffic.
<!--ID: 1748883231831-->


---

Q:  
Why might a program run faster after multiple executions on the same data?  
A:  
Subsequent executions may benefit from a warm cache, where data and instructions are already present in the CPU cache or the disk cache in RAM. This reduces the number of cold cache misses and disk I/O, leading to faster memory access and shorter execution time. Modern systems also optimize branch predictors and CPU prefetchers over time, improving performance further.
<!--ID: 1748883231833-->


Q:  
How can loop unrolling affect cache performance?  
A:  
Loop unrolling reduces the overhead of loop control logic and increases instruction-level parallelism, but it can also increase the instruction cache footprint. If unrolled too much, it can lead to instruction cache thrashing. On the data side, unrolling may help by aligning memory access patterns better with cache lines, improving spatial locality, especially in tight loops processing arrays.
<!--ID: 1748883231836-->


Q:  
How does cache line alignment affect SIMD operations?  
A:  
SIMD instructions load and store data in fixed-width chunks such as 128, 256, or 512 bits. If these memory accesses are not aligned to the corresponding cache line or SIMD register width, the CPU may need to perform multiple memory accesses to retrieve a single SIMD load. This degrades performance and can require software alignment fixes or special unaligned load instructions that are typically slower.
<!--ID: 1748883231838-->


Q:  
What is the purpose of the `__builtin_prefetch` function in GCC and when should it be used?  
A:  
`__builtin_prefetch` hints to the compiler and CPU that a particular memory address will be accessed soon, allowing the CPU to start loading it into the cache early. It’s useful in situations where hardware prefetchers don’t work well, such as when traversing linked lists or accessing data with irregular patterns. Overuse or poorly timed prefetching can lead to cache pollution or wasted memory bandwidth.
<!--ID: 1748883231841-->


Q:  
How do page faults interact with cache behavior?  
A:  
A page fault occurs when a program accesses a virtual memory address that is not currently mapped to a physical page. Resolving it may involve loading data from disk and mapping it into memory, which is extremely slow. While TLB misses are usually fast if the page tables are in RAM, a full page fault bypasses the cache hierarchy entirely and incurs a massive performance penalty. High-performance systems work to avoid page faults entirely in latency-critical paths.
<!--ID: 1748883231843-->


---

Q:  
Can cache misses be entirely eliminated by using larger caches?  
A:  
No, cache misses cannot be entirely eliminated. While larger caches reduce the likelihood of capacity misses, conflict misses and compulsory (first-time) misses will still occur. Moreover, larger caches tend to be slower and more power-hungry, and some workloads exhibit poor locality regardless of cache size. Optimizing data layout and access patterns remains critical even with large caches.
<!--ID: 1748883231845-->


---

Q:  
What is the impact of cache associativity on conflict misses and lookup latency?  
A:  
Higher associativity (e.g., 8-way vs. 2-way) reduces the chance of conflict misses because data can be placed in more locations within each set. However, this comes at the cost of increased hardware complexity and slightly longer lookup times, as more tags must be compared per access. There is a trade-off: low associativity is fast but has more conflict misses, while high associativity reduces conflicts but may slightly increase access latency.
<!--ID: 1748883231848-->


---

Q:  
How does memory interleaving improve bandwidth utilization in multi-core systems?  
A:  
Memory interleaving distributes consecutive chunks of memory across multiple physical memory banks or channels. This allows concurrent access to different memory banks by multiple cores, increasing overall throughput. It also reduces the likelihood of all cores contending for the same memory channel. When combined with good cache behavior, interleaving can significantly improve effective memory bandwidth in high-performance systems.
<!--ID: 1748883231851-->
