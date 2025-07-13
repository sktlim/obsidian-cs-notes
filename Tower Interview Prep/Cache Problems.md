TARGET DECK: Interview Prep::Cache Problems

Q:  
What is false sharing and why is it detrimental to performance?  
A:  
False sharing occurs when multiple threads on different CPU cores modify distinct variables that happen to reside on the same cache line. Even though the threads are not actually sharing data, the cache coherence protocol treats it as a conflict and invalidates the line across cores. This leads to unnecessary cache line transfers, high latency, and poor scalability. Padding variables or aligning them to cache line boundaries is a common way to avoid false sharing.
<!--ID: 1748883231642-->


---

Q:  
What is cache thrashing and how can it occur in a program?  
A:  
Cache thrashing happens when a program frequently accesses data that maps to the same cache sets, causing continual evictions and reloading of cache lines. This often arises in low-associativity caches or in tight loops with poor memory stride patterns. It leads to a high miss rate despite relatively small working sets. Changing the data layout or using padding to spread accesses across different cache sets can reduce thrashing.
<!--ID: 1748883231645-->


---

Q:  
How do TLB misses affect cache and memory performance?  
A:  
The TLB (Translation Lookaside Buffer) caches virtual-to-physical address translations. A TLB miss requires a page table walk, which consumes multiple memory accesses. Although this doesn't directly affect cache data, it delays the actual memory access, increasing effective memory latency. Frequent TLB misses degrade performance, especially when accessing large arrays without locality. Using larger page sizes (hugepages) can reduce TLB misses.
<!--ID: 1748883231647-->


---

Q:  
How can loop stride affect cache efficiency?  
A:  
Loop stride determines how data is accessed in memory. A stride of 1 (sequential access) aligns well with cache lines and benefits from spatial locality. Larger strides can skip over cache lines, leading to poor utilization and increased cache misses. For example, accessing every 64th element in an array means each access loads a new cache line, wasting the rest of its contents and causing frequent evictions.
<!--ID: 1748883231650-->


---

Q:  
Why is sharing a read-only data structure across threads safe from cache contention?  
A:  
Read-only data does not trigger cache invalidations because there are no writes that require coherence updates. Multiple threads can safely read from the same cache line without causing false sharing or performance degradation. This is one reason why immutable or read-mostly data structures scale well in multi-threaded systems.
<!--ID: 1748883231652-->


---

Q:  
What is a capacity miss and when does it become the bottleneck?  
A:  
A capacity miss occurs when the working set of a program exceeds the total size of the cache. In this case, even with perfect data layout, cache lines are constantly evicted and reloaded due to insufficient capacity. This is common in large matrix operations, large hash tables, or deep recursive functions. Optimizing algorithms to reduce active data size or breaking workloads into cache-sized tiles can help mitigate capacity misses.
<!--ID: 1748883231655-->


---

Q:  
Can instruction cache misses affect data processing performance?  
A:  
Yes. If the instruction cache (I-cache) misses frequently—such as during execution of large functions with poor locality or excessive branching—the CPU must fetch instructions from slower memory, stalling execution. This delays even data processing tasks. Splitting large functions, reducing dynamic dispatch, and inlining hot paths can help reduce I-cache pressure.
<!--ID: 1748883231657-->


---

Q:  
How does associativity affect the likelihood of conflict misses?  
A:  
Higher associativity allows multiple cache lines to occupy the same set, reducing the chance of two active addresses evicting each other. In low-associativity caches, multiple accesses to addresses that hash to the same set will result in frequent evictions and reloads. However, increasing associativity also increases lookup latency and hardware complexity, so it's a trade-off managed by CPU designers.
<!--ID: 1748883231660-->


---

Q:  
What role does data alignment play in preventing cache-related issues?  
A:  
Data alignment ensures variables are stored at memory addresses that match their natural size (e.g., 4-byte variables aligned to 4-byte boundaries). Misaligned data may cross cache line boundaries, requiring multiple fetches for one access, reducing performance. Proper alignment also avoids false sharing by allowing better control over cache line placement.
<!--ID: 1748883231663-->


---

Q:  
What is cache pollution and how can it be mitigated?  
A:  
Cache pollution happens when infrequently used or one-time-use data is loaded into the cache, evicting more useful data. This degrades overall cache efficiency. Cache pollution can be mitigated by using non-temporal loads/stores (which bypass the cache), software prefetching only when beneficial, and avoiding unnecessary memory accesses in hot paths.
<!--ID: 1748883231665-->


Q:  
Why does increasing the number of threads sometimes degrade performance in a multicore program?  
A:  
As thread count increases, more cores access and potentially modify shared data. This raises the chances of **false sharing**, **cache line bouncing**, and **TLB pressure**. Threads may also compete for shared cache (L2/L3), leading to higher eviction rates. If data structures are not properly partitioned, the overhead from cache coherence and contention can outweigh parallelization benefits.
<!--ID: 1748883231668-->


---

Q:  
What is a ping-pong effect in multi-threaded programs?  
A:  
The ping-pong effect refers to frequent ownership transfers of a shared cache line between cores, typically due to concurrent writes. Every time one core writes to a line, it must invalidate the copy on another core and fetch exclusive access, introducing latency and increasing coherence traffic. This results in oscillating access patterns and degraded performance. Avoiding shared writable data structures or using thread-local buffers helps eliminate this.
<!--ID: 1748883231671-->


---

Q:  
How do large data structures like hash maps or matrices contribute to cache pressure?  
A:  
Large data structures often exceed the size of CPU caches. If their access pattern is irregular or lacks spatial locality, each access may touch a new cache line, leading to **capacity misses**. Hash maps have particularly poor locality due to their random access behavior, and matrices accessed by column instead of row (in row-major layout) can cause strided access issues. Optimizing layout and access order is crucial to reducing misses.
<!--ID: 1748883231674-->


---

Q:  
What happens when two variables share the same cache line but are updated by separate threads?  
A:  
Even if the two variables are unrelated, sharing a cache line leads to **false sharing**. Writes by one thread cause the other thread’s core to invalidate and reload the cache line, even though the data of interest hasn't changed. This causes unnecessary traffic and can significantly reduce throughput in parallel applications. Introducing padding between variables can separate them into distinct lines and resolve the issue.
<!--ID: 1748883231677-->


---

Q:  
Why does frequent dynamic memory allocation affect cache performance?  
A:  
Dynamic memory allocation often scatters data across the heap, breaking spatial locality. Accesses to such fragmented data prevent efficient cache line usage and increase the chance of **TLB misses** and **cache misses**. Moreover, heap allocators may return poorly aligned memory, exacerbating alignment issues. Using memory pools or arena allocators that provide contiguous blocks helps maintain locality.
<!--ID: 1748883231679-->


---

Q:  
How can read-modify-write operations cause coherence overhead?  
A:  
Read-modify-write operations like `x++` require exclusive access to a cache line. Even if the line is shared across cores for reads, the modifying thread must first invalidate others’ copies. This triggers a coherence protocol update, causing performance degradation if such operations are frequent or shared across many threads. Redesigning algorithms to use thread-local accumulators reduces this contention.
<!--ID: 1748883231682-->


---

Q:  
What is the performance impact of random access patterns in arrays or vectors?  
A:  
Random access eliminates spatial locality, causing each access to potentially load a different cache line. This leads to high cache miss rates, increased memory bandwidth usage, and stalls while waiting for data to be fetched from lower levels of memory. In contrast, sequential access allows hardware prefetchers and cache lines to work efficiently. Random access is especially problematic in tight loops or large datasets.
<!--ID: 1748883231684-->


---

Q:  
Why might inserting debugging statements like `printf` mask performance issues in caches?  
A:  
`printf` introduces I/O delays, which can incidentally space out accesses to shared or conflicting cache lines. This unintentionally reduces contention and makes problems like false sharing or cache thrashing less visible. When `printf` is removed, the original issue may return. This is why performance bugs must be diagnosed using profiling tools rather than relying on debug output behavior.
<!--ID: 1748883231687-->


---

Q:  
How does increasing the associativity of a cache reduce conflict misses?  
A:  
Higher associativity allows each cache set to store more entries, reducing the likelihood that multiple frequently accessed addresses will evict each other. For example, in a 4-way associative cache, any of 4 lines in a set can be used for new data, as opposed to just one in a direct-mapped cache. This flexibility reduces the chance of conflict misses, although lookup time may slightly increase.
<!--ID: 1748883231690-->


---

Q:  
Can compiler optimizations affect cache behavior?  
A:  
Yes. Compiler optimizations such as loop unrolling, vectorization, and reordering memory accesses can improve or worsen cache usage. For example, aggressive inlining and loop fusion may increase instruction cache pressure, while loop tiling can improve data locality. Understanding how these transformations affect memory layout and access patterns is essential when tuning for cache performance.
<!--ID: 1748883231693-->


Q:  
How can hardware prefetchers degrade performance in low-latency systems?  
A:  
Hardware prefetchers attempt to predict upcoming memory accesses and load data into cache ahead of time. While effective for regular access patterns like linear arrays, they can mispredict with irregular structures like linked lists, trees, or strided access. These false positives waste memory bandwidth and pollute the cache by evicting useful data. In low-latency systems, this can introduce jitter and unexpected slowdowns. Developers may disable prefetching or favor software-controlled prefetches for better control.
<!--ID: 1748883231696-->


---

Q:  
What is a non-temporal store and when should it be used?  
A:  
Non-temporal stores bypass the cache and write data directly to memory, avoiding cache pollution. They are useful for data that will not be reused soon, such as streaming writes or large buffer initializations. On x86 architectures, instructions like `MOVNTDQ` (SSE) or `MOVNTI` (integer) perform such operations. Misuse of non-temporal stores—like writing small chunks—can actually reduce performance, so they should be reserved for large, linear memory operations.
<!--ID: 1748883231698-->


---

Q:  
What is an inclusive cache and how can it cause unexpected evictions?  
A:  
In an inclusive cache hierarchy, data in L1 or L2 is guaranteed to also exist in L3. If L3 evicts a line, the same line is forcibly evicted from the lower levels as well. This can evict actively used data from L1/L2, causing performance degradation due to unnecessary cache misses. Inclusive caches simplify coherence across cores but trade off against locality preservation in deeper cache levels.
<!--ID: 1748883231701-->


---

Q:  
What is page coloring and how does it reduce cache conflicts?  
A:  
Page coloring (or cache coloring) is a technique used by memory allocators to distribute memory pages across different cache sets. Since physical memory addresses determine cache set mappings, poorly randomized allocations can lead to many active pages mapping to the same set, increasing conflict misses. Coloring ensures that allocations avoid overlapping cache sets. While not user-accessible in most OSes, it’s used in RTOS and low-latency environments to improve predictability.
<!--ID: 1748883231703-->


---

Q:  
How does cache behavior change in a virtualized environment like a cloud VM?  
A:  
In virtualized environments, multiple VMs or containers may share the same physical CPU, cache, and TLBs. The hypervisor may schedule other workloads on the same core, evicting your cache lines and causing unpredictable performance. Additionally, TLB entries can be flushed on context switches. On CPUs with Intel CAT (Cache Allocation Technology), it's possible to allocate portions of the L3 cache to isolate workloads, but this is rarely available on cloud instances.
<!--ID: 1748883231706-->


---

Q:  
What causes instruction cache (I-cache) misses and how can they be reduced?  
A:  
I-cache misses occur when the instruction stream is too large or poorly localized, leading to evictions of recently executed code. This can happen with huge functions, deep inlining, virtual dispatch, or frequent jumps to distant code sections. To reduce I-cache pressure, hot paths can be grouped together using link-time optimization (LTO), manual function placement, or function splitting. Keeping inner loops small and avoiding bloated dispatch logic also helps.
<!--ID: 1748883231709-->


---

Q:  
Why is it important to consider cache miss penalty and not just miss rate?  
A:  
Not all cache misses have equal cost. A miss that is resolved in L2 is much faster than one that goes to main memory. High miss rates may not hurt if they are resolved quickly (e.g., L1 to L2), but low miss rates with high penalties (e.g., L1 to DRAM) can cripple performance. Profilers like `perf` or Intel VTune help distinguish between miss types, guiding which part of the hierarchy to optimize.
<!--ID: 1748883231712-->


---

Q:  
How do software prefetches differ from hardware prefetches?  
A:  
Software prefetches are explicit instructions added by the programmer to hint that certain memory will be used soon. They offer precise control but require accurate tuning to balance timing. Too early, and the data may be evicted before use. Too late, and the benefit is lost. Unlike hardware prefetchers, which are automatic but opaque, software prefetches can be customized to irregular access patterns, making them useful in hand-tuned low-latency code.
<!--ID: 1748883231714-->


---

Q:  
What is the effect of aggressive function inlining on the instruction cache?  
A:  
Function inlining reduces call overhead and can expose optimization opportunities, but excessive inlining increases binary size and instruction cache footprint. This can evict hot code paths from I-cache and degrade performance. In latency-critical loops, keeping code compact ensures better I-cache residency. Compilers usually balance inlining based on heuristics, but manual control using `__attribute__((noinline))` or `inline` keywords may be necessary in performance tuning.
<!--ID: 1748883231717-->


---

Q:  
How can cache-aware memory allocators improve performance in systems programming?  
A:  
Cache-aware allocators can align memory to cache line boundaries, separate frequently accessed objects to different lines, and avoid reuse patterns that lead to cache conflicts. This reduces false sharing, improves locality, and minimizes conflict and capacity misses. In trading systems, where memory patterns are often repeatable and performance-sensitive, using slab allocators, object pools, or even custom memory arenas tuned for access patterns can dramatically reduce tail latency.
<!--ID: 1748883231719-->


Q:  
What are speculative cache misses and why can they distort performance counter readings?  
A:  
Speculative execution means the CPU may fetch and execute instructions out of order or along a predicted path. During this speculation, it may load data into cache that is never actually used. Performance counters often record these speculative cache loads as “misses,” even if the speculation is discarded. This can inflate cache miss rates and mislead developers during profiling. Using precise event-based sampling (PEBS) or filtering with `:p` modifiers in `perf` can help reduce misattribution.
<!--ID: 1748883231722-->


---

Q:  
What is a write-combining buffer and how does it optimize memory throughput?  
A:  
Write-combining buffers aggregate small, sequential stores into a single larger write to memory, reducing bus traffic and improving bandwidth. These are especially beneficial in non-temporal or streaming write scenarios, like zeroing a large memory block or logging. However, non-sequential or interleaved writes may flush the buffer prematurely, negating its benefits. Systems programmers must ensure writes are aligned and sequential to benefit from write combining.
<!--ID: 1748883231725-->


---

Q:  
What is the role of memory fences in maintaining cache consistency, and why are they expensive?  
A:  
Memory fences (e.g., `mfence`, `sfence`, `lfence` on x86) enforce ordering constraints on memory operations. They prevent the CPU and compiler from reordering reads/writes around the fence. In multithreaded systems, fences ensure that updates are visible in a consistent order across cores. However, they flush the pipeline and stall execution, introducing significant latency. Overuse can cripple performance, so they're typically reserved for synchronization primitives or volatile hardware I/O.
<!--ID: 1748883231727-->


---

Q:  
How do DTLB and ITLB misses affect cache behavior differently?  
A:  
DTLB (Data TLB) handles virtual-to-physical address translation for data accesses, while ITLB (Instruction TLB) does the same for instruction fetches. DTLB misses usually occur in large or scattered data structures like hash maps, while ITLB misses are more common in large functions, switch-heavy dispatch code, or deep inlined binaries. DTLB misses hurt data throughput; ITLB misses stall instruction decoding and may disrupt branch prediction. Tools like `perf` can distinguish between these and inform whether to optimize code or data layout.
<!--ID: 1748883231730-->


---

Q:  
What are cache bank conflicts, and how are they different from cache set conflicts?  
A:  
Cache bank conflicts occur when multiple memory accesses target the same internal **bank** within a cache level (usually L1), even if they map to different sets. This can cause access serialization and pipeline stalls. Unlike cache set conflicts (which lead to evictions), bank conflicts hurt performance without triggering misses. They are architecture-specific and usually affect vectorized or simultaneous memory loads. Avoiding simultaneous access to addresses with specific strides can help mitigate this.
<!--ID: 1748883231733-->


---

Q:  
How can function inlining increase instruction cache pressure, and how can this be diagnosed?  
A:  
Inlining replaces function calls with the actual function body, reducing call overhead and improving locality for small functions. However, excessive inlining increases binary size and causes hot code to exceed instruction cache capacity. This leads to I-cache misses and stalls. Diagnosing this involves checking for high ITLB or I-cache miss counts via `perf` or `vtune`. Solutions include selectively disabling inlining (`noinline`), or refactoring large switch-heavy functions into smaller, cache-friendly blocks.
<!--ID: 1748883231736-->


---

Q:  
Why do SMT (Simultaneous Multithreading) and shared L1 caches pose a problem for cache predictability?  
A:  
In SMT (e.g., Intel Hyper-Threading), two logical cores share certain resources, including L1 cache and TLBs. If both threads access memory heavily, they evict each other's cache lines and pollute the TLB, causing unpredictable latency. Additionally, one thread may trigger cache coherence traffic that stalls the other. In latency-critical environments, disabling SMT or pinning critical threads to isolated cores avoids such interference and improves determinism.
<!--ID: 1748883231742-->


---

Q:  
How does the OS affect cache usage during context switches?  
A:  
During a context switch, the OS may reschedule a thread on a different core. Since caches are core-local, this causes cold cache effects—known as "cache migration penalties." TLB entries may also be invalidated, leading to page table walks. For low-latency systems, thread affinity (`sched_setaffinity`) and kernel parameters like `isolcpus` help keep threads pinned to the same core, preserving cache locality and reducing jitter.
<!--ID: 1748883231744-->


---

Q:  
What is Intel Cache Allocation Technology (CAT) and how can it help low-latency applications?  
A:  
CAT allows fine-grained control over how shared L3 cache is allocated among different cores or processes. It works by defining classes of service (CLOS) that map tasks to specific cache ways. This reduces cache contention and improves isolation, especially in virtualized or containerized environments. While not widely supported in public clouds, CAT is valuable in colocated infrastructure to prevent non-trading processes from evicting latency-critical data.
<!--ID: 1748883231747-->


---

Q:  
How can `memcpy` or `memset` behave sub-optimally in critical code paths?  
A:  
Standard library implementations of `memcpy` and `memset` are optimized for general-purpose throughput. However, in latency-sensitive scenarios, they may introduce unpredictability due to alignment handling, fallback paths, or vectorization. For instance, copying unaligned data may trigger multiple loads/stores or bypass vectorized instructions. In trading systems, developers often use custom, alignment-aware versions of these routines to ensure consistent latency in performance-critical code.
<!--ID: 1748883231750-->
