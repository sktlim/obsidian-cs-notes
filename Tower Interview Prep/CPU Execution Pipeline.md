TARGET DECK: Interview Prep::CPU Execution

Q:  
What is a CPU pipeline and why is it important for performance?  
A:  
A CPU pipeline breaks down instruction execution into stages (like fetch, decode, execute, memory access, write-back), allowing multiple instructions to be processed concurrently — one per stage per cycle. This increases throughput significantly. In low-latency systems, understanding how to avoid stalls or pipeline flushes is crucial, since even a single disruption can delay subsequent instructions and impact critical path timing.
<!--ID: 1748883231516-->


---

Q:  
What causes a pipeline stall and how can it be avoided?  
A:  
A pipeline stall happens when the CPU must wait before executing the next instruction, typically due to data hazards (e.g., needing a result that isn’t ready), control hazards (branches), or resource conflicts. Stalls waste CPU cycles. They can be avoided by reordering instructions to eliminate dependencies, using branch prediction to hide control hazards, and leveraging instruction-level parallelism to keep the pipeline full.
<!--ID: 1748883231523-->


---

Q:  
What is instruction-level parallelism (ILP) and how does it relate to latency optimization?  
A:  
Instruction-level parallelism refers to the CPU's ability to execute multiple independent instructions simultaneously in the pipeline. Superscalar processors dispatch several instructions per clock cycle if there are no dependencies. Writing code with minimal inter-instruction dependencies and avoiding serial logic (like dependent arithmetic) enables better ILP, which reduces the number of cycles per operation and improves execution latency.
<!--ID: 1748883231526-->


---

Q:  
What is a branch misprediction and why is it expensive?  
A:  
Modern CPUs use branch predictors to guess the outcome of conditional branches to avoid stalling the pipeline. If the guess is wrong (misprediction), the CPU must discard all speculatively executed instructions and restart from the correct path. This causes a pipeline flush and costs 10–20 cycles or more. In performance-critical paths, avoiding unpredictable branches or replacing them with predicated operations can help reduce this penalty.
<!--ID: 1748883231529-->

Q:  
How do out-of-order execution engines reduce latency?  
A:  
Out-of-order execution allows the CPU to execute instructions as soon as their operands are ready, rather than strictly in program order. This helps hide latencies caused by memory loads or earlier instructions waiting on data. The CPU tracks dependencies and ensures final results appear in-order. It improves throughput and reduces visible latency, especially in code with some idle gaps that can be filled with independent instructions.
<!--ID: 1748883231534-->


---

Q:  
What is register renaming and how does it prevent pipeline stalls?  
A:  
Register renaming eliminates false dependencies by mapping logical registers in the instruction stream to physical registers in hardware. This allows independent instructions to proceed even if they use the same register names. It prevents write-after-read (WAR) and write-after-write (WAW) hazards, enabling better instruction scheduling and utilization of execution units.
<!--ID: 1748883231537-->


---

Q:  
How can developers write code that minimizes pipeline disruption?  
A: They can: Avoid long dependency chains (e.g., avoid `a = f(g(x))` if f and g are expensive). Eliminate unpredictable branches (e.g., use lookup tables or conditional moves). Minimize use of shared functional units (e.g., divide and square root units). Favor simple arithmetic and sequential memory access. Reorder instructions manually or using compiler intrinsics to expose ILP. This reduces stalls and makes better use of the CPU's parallel execution capabilities.
<!--ID: 1748883445426-->


---

Q:  
What role does speculative execution play in pipeline efficiency?  
A:  
Speculative execution allows the CPU to execute instructions before it knows whether they're needed, based on predicted paths or load speculation. If speculation is correct, performance improves; if incorrect, the speculative instructions are discarded. It increases overall throughput but can lead to wasted work or security concerns (e.g., Spectre). In trading systems, it can cause cache pollution or timing variation if misused.
<!--ID: 1748883231548-->


---

Q:  
How do fused multiply-add (FMA) instructions benefit pipeline execution?  
A:  
FMA instructions perform a multiplication and addition in a single pipeline operation (e.g., `a = b * c + d`). This reduces instruction count, saves register usage, and increases arithmetic throughput. Since FMA uses a dedicated execution path on many CPUs, leveraging it can lead to better pipeline efficiency and lower latency in math-heavy workloads like pricing or signal processing.
<!--ID: 1748883231554-->


Q:  
What is a pipeline flush and what causes it?  
A:  
A pipeline flush occurs when the CPU discards all instructions currently in the pipeline, typically due to a branch misprediction, exception, or serializing instruction. This resets the flow of instruction execution and incurs a latency penalty equal to the depth of the pipeline. In latency-sensitive systems, pipeline flushes should be minimized through predictable branching, limited speculation, and exception-free hot paths.
<!--ID: 1748883231560-->


---

Q:  
What is the role of micro-operations (µops) in modern CPU pipelines?  
A:  
Modern CPUs break complex instructions down into simpler micro-operations (µops) that can be independently scheduled and executed. This abstraction allows parallel dispatch and execution on superscalar CPUs. However, some CISC instructions (e.g., `rep movsb`, string ops) expand into many µops and may overwhelm scheduling queues. Knowing the µop cost of an instruction helps developers write faster, more schedulable code.
<!--ID: 1748883231566-->


---

Q:  
How does micro-op fusion improve pipeline efficiency?  
A:  
Micro-op fusion combines certain instruction pairs (e.g., `cmp` + `jmp`) into a single µop internally, reducing dispatch and decode bandwidth usage. This allows more instructions to pass through the pipeline per cycle and reduces pressure on decode stages. Using patterns that trigger fusion (like `cmp` + conditional branch instead of `test` + setflag) helps improve control-flow throughput.
<!--ID: 1748883231572-->


---

Q:  
Why are dependent loads a major bottleneck in instruction pipelines?  
A:  
A dependent load is a memory read whose address depends on the result of a previous instruction. The CPU cannot execute the load until the address is computed, causing a pipeline stall. In deep memory hierarchies, this can delay execution by tens to hundreds of cycles. Rearranging code to decouple computations or precompute addresses improves pipeline flow.
<!--ID: 1748883231578-->


---

Q:  
What is port contention and how can it impact pipeline execution?  
A:  
Modern CPUs have multiple execution ports that handle different types of µops (e.g., ALU ops, memory loads, branches). Port contention occurs when too many instructions target the same execution unit or port, leading to queuing and stalled issue cycles. Profiling tools like Intel’s IACA or `perf` can reveal port pressure, and code can be optimized to balance instructions across multiple ports.
<!--ID: 1748883231584-->


---

Q:  
What are serializing instructions and why are they costly in pipelines?  
A:  
Serializing instructions force the CPU to complete all prior instructions and drain the pipeline before continuing. Examples include `CPUID`, `RDMSR`, and `LFENCE`. These are used for precise timing or I/O access but break out-of-order and speculative execution, causing pipeline stalls. In low-latency paths, they should be avoided unless absolutely necessary for correctness.
<!--ID: 1748883231587-->


---

Q:  
How does simultaneous multithreading (SMT) affect pipeline resource sharing?  
A:  
In SMT (e.g., Hyper-Threading), multiple threads share pipeline resources like fetch/decode stages, execution units, and caches. If both threads are active and issue heavy instructions, they compete for the same ports and units, reducing available throughput for each. In performance-sensitive applications, disabling SMT or isolating critical threads to dedicated cores improves predictability.
<!--ID: 1748883231590-->


---

Q:  
How can instruction scheduling be influenced manually in low-level code?  
A:  
Manual instruction scheduling reorders instructions to reduce stalls from dependencies or port contention. This is often done using compiler intrinsics, inline assembly, or even hand-tuned binaries. Developers align loads and arithmetic in a staggered pattern to avoid back-to-back dependencies. On CPUs with known pipelines, matching instruction sequences to port mappings (e.g., ALU vs memory vs FPU) helps maximize instruction throughput.
<!--ID: 1748883231593-->


---

Q:  
Why does deep speculation depth matter in pipeline design for trading systems?  
A:  
A deeper speculation window allows the CPU to execute more instructions ahead of time, which improves throughput in general-purpose computing. However, in trading systems, this increases **latency variance**, especially when speculation fails (e.g., unpredictable branches). The deeper the speculation, the more instructions are flushed if a branch mispredicts, introducing tail latency that is unacceptable in HFT contexts.
<!--ID: 1748883231595-->


---

Q:  
How can measuring IPC (Instructions Per Cycle) reveal pipeline inefficiencies?  
A:  
IPC indicates how many instructions the CPU retires per clock cycle. A high IPC (~4 for many Intel cores) means good utilization, while a low IPC suggests stalls, mispredictions, or memory bottlenecks. If IPC drops under load, the program may be memory-bound or have poor instruction scheduling. Tools like `perf stat` help track IPC and correlate it with cache misses or branch mispredictions to guide optimization.
<!--ID: 1748883231598-->


Q:  
What is decode bandwidth, and why does it matter in low-latency systems?  
A:  
Decode bandwidth is the maximum number of instructions the CPU can decode per cycle. On many x86 CPUs, this is limited to 4–6 instructions. If the instruction stream contains complex or variable-length instructions, the decode stage can become the bottleneck, even if execution units are underutilized. This affects throughput in tight loops and latency in hot paths, making it important to write compact, simple instruction sequences.
<!--ID: 1748883231601-->


---

Q:  
How can instruction choice affect decode throughput?  
A:  
Complex instructions like string operations (`rep movsb`, `stosb`), floating-point math, and some vector operations can expand into multiple µops, consuming more decode slots. Replacing them with simpler equivalents or hand-unrolling loops may improve decode efficiency. Additionally, using short, fixed-width instructions helps avoid decode bottlenecks and improves frontend utilization.
<!--ID: 1748883231604-->


Q:  
What are load/store buffers and how do they impact pipeline performance?  
A:  
Load and store buffers track memory operations that are in-flight (not yet completed). Each buffer has a finite number of entries (e.g., 72 loads, 44 stores on Skylake). If these buffers fill up due to memory latency or high instruction rate, new memory operations must stall, blocking pipeline progress. Even with good cache hit rates, too many outstanding loads or stores can saturate the buffers and hurt throughput.
<!--ID: 1748883231607-->


---

Q:  
What kind of workloads are especially prone to saturating load/store buffers?  
A:  
Streaming, memory-bound workloads that perform bulk loads or stores (e.g., matrix processing, hash table probes, bulk data copies) can easily fill load/store buffers. This is especially problematic if loads depend on prior memory lookups or if stores are unaligned or non-coalesced. Proper pacing of memory operations, loop unrolling, and prefetching can help alleviate pressure on these buffers.
<!--ID: 1748883231609-->


Q:  
What is the Reorder Buffer and what happens when it is full?  
A:  
The ROB holds instructions that have been dispatched but not yet retired, maintaining in-order visibility of results. It ensures that out-of-order execution does not violate program order. When the ROB is full (e.g., due to a stalled long-latency load), the frontend stops issuing new instructions. This causes a temporary stall in instruction dispatch and reduces pipeline throughput until space is freed.
<!--ID: 1748883231612-->


---

Q:  
How can you reduce the chance of ROB saturation in latency-sensitive code?  
A:  
You can reduce ROB pressure by avoiding long dependency chains and reducing the number of outstanding memory operations. Splitting long sequences of dependent instructions into independent chains allows instructions to retire earlier. Memory loads that may miss in cache should be isolated from the hot path using software pipelining, speculative loads, or prefetching.
<!--ID: 1748883231614-->


Q:  
How can prefetching introduce pipeline inefficiencies?  
A:  
While prefetching hides memory latency, it adds µops that must pass through the frontend. If too many prefetch instructions are inserted—especially in tight loops—they can consume decode and dispatch bandwidth. In addition, aggressive prefetching can cause unnecessary memory reads, polluting caches and increasing power and bus traffic. Prefetching should be used judiciously and tuned for timing.
<!--ID: 1748883231617-->


---

Q:  
What is the difference between hardware and software prefetching in terms of pipeline pressure?  
A:  
Hardware prefetching is automatic and non-blocking but may interfere with useful memory operations if misconfigured. Software prefetching generates explicit instructions (like `__builtin_prefetch`), which must be decoded, dispatched, and executed, consuming pipeline resources. In low-latency paths, misplaced software prefetches can choke the frontend, so they should be carefully profiled and aligned with the memory access pattern.
<!--ID: 1748883231619-->


Q:  
What is the µop cache and how does it reduce latency?  
A:  
The µop cache stores previously decoded instructions (as micro-operations), allowing the CPU to bypass the traditional decode stage for repeated code paths. If the instruction stream hits in the µop cache, the decoded µops are directly fed into the execution backend, reducing latency and saving power. This is especially beneficial for small, hot loops or frequently executed functions.
<!--ID: 1748883231622-->


---

Q:  
How can you ensure that performance-critical code benefits from the µop cache?  
A:  
Keep hot functions small and avoid excessive inlining that would bloat their footprint. Avoid inserting rarely executed slow paths into hot functions, which may evict the loop from the µop cache. Use tools like Intel VTune or IACA to inspect whether loops are µop cache-resident, and restructure code to isolate cold logic from the tight loop.
<!--ID: 1748883231624-->


Q:  
What are hardware loop buffers and when are they used?  
A:  
Hardware loop buffers (found on ARM and some embedded CPUs) cache instructions for small loops directly inside the execution engine, allowing zero-latency loop iteration without instruction fetch or decode. This greatly improves energy and cycle efficiency for tight loops. While not present in most x86 CPUs, understanding this mechanism is useful when working with heterogeneous systems or FPGAs with soft cores.
<!--ID: 1748883231626-->


---

Q:  
Why are hardware loop buffers less relevant on x86, and how do you achieve similar benefits?  
A:  
x86 CPUs rely more on µop caches and aggressive out-of-order execution rather than HLBs. To achieve similar benefits, keep loops tight and aligned, reduce instruction count, and fit the entire loop within the µop cache. This reduces frontend pressure and enables high IPC execution across iterations.
<!--ID: 1748883231629-->


Q:  
What is control flow fusion and how does it optimize the pipeline?  
A:  
Control flow fusion allows the CPU to fuse certain instruction pairs (like a compare followed by a conditional jump) into a single µop. This reduces the decode and dispatch load and improves branch prediction accuracy. Writing code that uses `cmp` followed immediately by a conditional branch (instead of more complex logic) can help the CPU perform this fusion.
<!--ID: 1748883231632-->


---

Q:  
When should you use conditional moves (`cmov`) instead of branches?  
A:  
Use `cmov` when the decision condition is unpredictable or when both paths are simple. Unlike branches, `cmov` is not subject to misprediction and does not cause pipeline flushes. However, it executes both source operands regardless of the condition, so avoid it if one path is computationally expensive or has side effects.
<!--ID: 1748883231634-->


Q:  
How do in-order and out-of-order pipelines differ in handling latency?  
A:  
In-order pipelines execute instructions strictly in program order, stalling whenever an instruction waits on data. Out-of-order (OoO) pipelines allow the CPU to execute independent instructions while waiting for slow ones to complete. OoO designs reduce visible stalls and are more forgiving of high-latency instructions. In contrast, in-order cores require carefully structured code with no hidden dependencies to avoid stalls.
<!--ID: 1748883231637-->


---

Q:  
Why does out-of-order execution benefit latency-sensitive code more than in-order?  
A:  
Out-of-order CPUs can hide the latency of memory loads and functional unit delays by scheduling independent instructions. This leads to better pipeline utilization and lower end-to-end latency. In contrast, in-order cores must wait for each instruction to complete before issuing the next, making them more prone to bubbles in the pipeline and dependent on compiler-level scheduling for efficiency.
<!--ID: 1748883231639-->

Q:  
What are the three main types of pipeline hazards and how do they affect execution?  
A:  
The three types are:
- **Data hazards**: Occur when instructions depend on the results of previous instructions (e.g., RAW hazards).
- **Control hazards**: Result from branches that change the flow of execution.
- **Structural hazards**: Occur when hardware resources (e.g., ALUs) are oversubscribed. Each type can delay execution by stalling the pipeline or forcing instruction reordering. Avoiding them involves careful instruction scheduling, minimizing branches, and using software pipelining techniques.
<!--ID: 1748883445429-->
