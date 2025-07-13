## Basics of Memory Hierarchy
- When a word is not found in the cache, it must be fetched from a lower level in the hierarchy and placed in the cache
- Multiple words (called a **block** or **line**) are moved for efficiency, and also due to spatial locality. Each cache block includes a tag to indicate which memory address it corresponds to

- Key design decision is where blocks can be placed in cache
	- Most popular scheme is **set associative**, where set is a group of blocks in the cache.
		- Block is first mapped onto a set, and then block can be placed anywhere within that set
		- Finding block consists of first mapping the block address to the set and then searching the set (usually in parallel) to find block
		- Set is chosen by address of data $$(Block \space Address) MOD (\text{Number of sets in cache})$$
		- If there are n blocks in set, cache placement is called **n-way set associative**
	- A **direct-mapped** cache has one block per set (so block is always in same location) and a **fully associative** cache has just one set (so block can be placed anywhere)

- Caching reads is easy since copy in cache and memory identical; caching writes is more challenging. There are 2 main strategies:
	- **Write-through cache**: Updates item in cache and writes through to update main memory
	- **Write-back cache**: Only updates copy in cache. When block is about to be replaced, its copied back into memory
- Both strategies can use **write buffer** to allow cache to proceed as soon as data placed in buffer rather than wait full latency to write data into memory
### 3 'C's model
Different cache organizations can be measured using **miss rate**: Fraction of cache accesses that result in a miss. The 3 Cs model sorts these misses into 3 categories.
1) **Compulsory**: First access to block cannot be in cache, so block must be brought into cache. Will occur even if cache is infinitely large
2) **Capacity**: If cache cannot contain all blocks needed during execution of program, capacity misses will occur because of blocks being discarded and later retrieved
3) **Conflict**: If block placement strategy is not fully associative, misses occur because block may be discarded and later retrieved if multiple blocks map to its set and accesses to different blocks are intermingled

Note: Multithreading and multiple cores add complications for caches, increasing the potential for capacity misses and adding a 4th C for **coherency** misses due to cache flushes to keep multiple caches coherent in multiprocessor.

Multithreading also allows a processor to tolerate misses without being forced to idle. Hence, cache must be able to service requests while handling a miss

## 6 Basic Cache Optimizations
1) **Larger block size reduces miss rate:** 
	- Utilize spatial locality and increase block size 
	- Reduces compulsory misses but increases capacity and conflict misses (especially for smaller caches) and miss penalty
2) **Bigger caches to reduce miss rate:**
	- Potentially longer hit time of larger cache and higher cost and power
	- Increases both static and dynamic power
3) **Higher associativity to reduce miss rate:**
	- Reduces conflict miss, but increases hit time
	- Increases power consumption
4) **Multilevel caches to reduce miss penalty:**
	- Makes higher level caches faster to match clock cycle and lower level caches larger to reduce gap between processor accesses and main memory accesses
	- More power efficient than single aggregate cache
5) **Giving priority to read misses over writes:**
	- Reduces miss penalty
	- Can implement in write buffer
		- Write buffers holds updated value of location needed on a read (read-after-write hazard)
	- Can check contents of write buffer on read miss
		- If no conflicts, and if memory system is available, sending read before writes reduces miss penalty 
6) **Avoiding address translation during indexing of cache to reduce hit time:**
	- Caches need to translate virtual address from processor to physical address so that it can access memory
	- Solution: Use page offset to index cache
		- Introduces some complications on size and structure of L1 cache, but removing TLB access from critical path outweighs disadvantages