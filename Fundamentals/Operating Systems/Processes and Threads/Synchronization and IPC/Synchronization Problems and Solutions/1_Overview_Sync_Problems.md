### Synchronization Problems and Solutions
- [[1_Overview_Sync_Problems]]
- [[Peterson's Solution for Race Conditions]]
- [[Priority Inversion Problem]]
- [[Producer-Consumer Problem]]
- [[Race conditions and Critical Region Solutions]]
- [[Read-Copy-Update (RCU)]]
- [[Solving race conditions without busy waiting]]
- [[The Readers and Writers Problem]]

TARGET DECK: Operating Systems::Processes and Threads::Synchronization Problems

Q: What is a race condition, and what causes it?
A: A race condition occurs when multiple threads access shared data simultaneously, and the result depends on the non-deterministic order of execution.
<!--ID: 1748149871973-->


Q: How does Peterson’s Solution achieve mutual exclusion?
A: It uses two shared variables (`flag[]` and `turn`) to ensure only one thread enters the critical section at a time, relying on busy waiting and memory visibility.
<!--ID: 1748149871977-->


Q: What is the main drawback of Peterson’s algorithm in modern systems?
A: It assumes strict memory ordering, which may not hold on modern CPUs without memory barriers; also, it's not scalable beyond two threads.
<!--ID: 1748149871979-->


Q: What is the producer-consumer problem, and how is it typically solved?
A: It involves coordinating producers and consumers accessing a shared buffer. Semaphores are used to track buffer slots and ensure mutual exclusion.
<!--ID: 1748149871983-->


Q: What is the critical region problem in concurrent systems?
A: It's the challenge of ensuring that only one thread executes in a critical section at a time to prevent data inconsistency or corruption.
<!--ID: 1748149871986-->


Q: What is the readers-writers problem, and why is it significant?
A: It models access control where multiple readers can access shared data simultaneously, but writers require exclusive access. It highlights issues of fairness and starvation.
<!--ID: 1748149871989-->


Q: What synchronization strategy solves race conditions without busy waiting?
A: Blocking primitives like mutexes, condition variables, and semaphores allow threads to sleep while waiting, avoiding CPU cycle wastage.
<!--ID: 1748149871993-->


Q: What is priority inversion, and how can it affect system responsiveness?
A: It's when a high-priority task is blocked by a low-priority task holding a needed resource, potentially leading to missed deadlines or hangs.
<!--ID: 1748149871996-->


Q: How can priority inversion be mitigated in real-time systems?
A: Using priority inheritance or priority ceiling protocols, which temporarily boost the priority of the blocking thread to resolve the inversion.
<!--ID: 1748149871999-->


Q: What is Read-Copy-Update (RCU) and where is it commonly used?
A: RCU is a synchronization mechanism that allows readers to access data without locking while updates are deferred or made safely. It’s widely used in the Linux kernel.
<!--ID: 1748149872001-->


Q: Why is RCU suitable for read-heavy workloads?
A: Because it minimizes synchronization overhead for readers by allowing concurrent, lock-free access, improving scalability.
<!--ID: 1748149872004-->

