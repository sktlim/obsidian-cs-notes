**Protection rings**: Different access levels to resources, from most privileged (ring 0) to least privileged
- Ring 0: Kernel 
- Ring 1: Device drivers (Modern OSes don't really use)
- Ring 2: Device drivers (Modern OSes don't really use)
- Ring 3: Applications

**Kernel Mode**: For OS to run; CPU may perform any operation allowed by architecture (Ring 0)

**User Mode:** For user programs to run; Certain instructions are prohibited e.g. IO operations, accessing certain parts of memory

**Multiplexing**: "Doing multiple things at the same time". Share single resource among multiple processes by dividing access in time (different programs take turns using the resource) and space (Each program gets part of the resource)

**Multiprogramming:** Keeps multiple programs in memory and context switches CPU between them to maximize CPU usage

**Multithreading:** Single process is divided into smaller, independent threads that run concurrently, allowing program to execute tasks simultaneously. Each thread shares program resources but can execute independently

| Concept                | McDonald's Metaphor                                                                 | Key Idea                                                        |
| ---------------------- | ----------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| Multiplexing           | Sharing resources efficiently, either by **time** or **space**                      | General resource sharing technique                              |
| **Time Multiplexing**  | One serving window; workers take turns quickly to serve customers                   | Sharing a single resource across time                           |
| **Space Multiplexing** | Multiple serving windows; each worker has their own to serve customers concurrently | Using multiple resources simultaneously                         |
| Multiprocessing        | Multiple kitchens cooking separate orders at the same time                          | True parallelism with multiple CPUs                             |
| Multiprogramming       | Several orders in progress by one waiter constantly switching between orders        | Switching between tasks in memory; Focus on max CPU utilization |
| Multithreading         | One worker preparing different items for a single combo meal                        | Concurrent tasks within one process                             |
| Multitasking           | Workers handling multiple customer orders, switching between them quickly           | Rapid task-switching by OS; Focus on responsive experience      |

**Spooling (Simultaneous peripheral operations on line)**: Temporarily storing data on buffer to be processed by a slower peripheral device once device is available

**POSIX**: Minimal system-call interface that conformant UNIX systems must support

**Hyperthreading**: Hardware-level technology allowing a single physical core to act as 2, each capable of handling its own thread, by duplicating its processor [[Hyperthreading]]

**Context switch**: Switching from 1 program to another

**Plug and Play (PnP)**: Allows a computer to automatically recognize and configure hardware components without requiring user intervention, driver installation, or manual configuration of resources like IRQs (Interrupt Requests), DMA (Direct Memory Access), or I/O addresses.

**Process:** Program in execution

**Spurious wakeup:** When thread wakes up from waiting on condition variable and finds that condition is still unsatisfied (awakened for no reason)

**Reincarnation Server**: Type of fault-tolerance mechanism used in distributed or highly reliable systems to monitor services/processes for failure and restart them to ensure continued operation

**Daemon:** Background processes

**Spinlock:** is a type of lock where a thread repeatedly checks if it can acquire the lock, "spinning" in place until it becomes available

**Convoy effect:** Short jobs wait for longer ones to complete

**Memory Hierarchy:** memory speed is tiered, with some memory being very fast (L1) and subsequent memory levels being slower in speed but larger in capacity

**Memory manager:** part of OS handling memory, allocation, deallocation

**Memory protection keys:** Hardware feature that allows fast, software-controlled access permissions for memory pages. Each page can be assigned a protection key, and access rights for these keys are managed by a register (PKRU) without modifying page tables, enabling efficient, dynamic permission changes for groups of memory pages

**Working Set:** Set of pages process is currently using

**Thrashing:** Program causing page fault every few instructions

