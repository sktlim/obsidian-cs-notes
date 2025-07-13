### Synchronization Primitives
- [[1_Overview_Sync_Primitives]]
- [[Condition Variables]]
- [[Difference Between a Mutexes, Semaphores and Condition Variables]]
- [[Futexes]]
- [[Monitors]]
- [[Mutexes]]
- [[Semaphores]]


## Flashcards

TARGET DECK: Operating Systems::Synchronization and IPC::Synchronization Primitives

Q: What problem do synchronization primitives solve in concurrent programming?
A: They prevent race conditions by coordinating access to shared resources among multiple threads or processes.
<!--ID: 1748186197872-->

Q: How does a mutex work?
A: A mutex provides mutual exclusion by allowing only one thread to access a critical section at a time; other threads must wait until the mutex is released.
<!--ID: 1748186197876-->

Q: How is a semaphore different from a mutex?
A: A semaphore can allow multiple concurrent accesses up to a defined limit (counting semaphore), whereas a mutex only allows one.
<!--ID: 1748186197879-->

Q: What’s a typical use case for a binary semaphore?
A: Enforcing mutual exclusion, similar to a mutex, but with less strict ownership guarantees.
<!--ID: 1748186197882-->

Q: What role do condition variables play in synchronization?
A: They allow threads to sleep and wait for specific conditions to become true, typically in conjunction with a mutex for signaling and coordination.
<!--ID: 1748186197885-->

Q: Why must condition variables be used with a mutex?
A: To avoid race conditions between checking the condition and waiting — the mutex ensures the check and wait are atomic.
<!--ID: 1748186197888-->

Q: What is a futex and how does it differ from traditional primitives?
A: A futex (fast userspace mutex) avoids kernel involvement for uncontested locks, improving performance by using a syscall only when necessary.
<!--ID: 1748186197891-->

Q: In what scenario would a monitor be more appropriate than manually using mutexes and condition variables?
A: When encapsulating both data and its synchronization mechanism (like in object-oriented concurrency), monitors simplify the design by bundling mutual exclusion and condition signaling.
<!--ID: 1748186197894-->


Q: How do semaphores help in producer-consumer problems?
A: One semaphore tracks the number of available slots (for producers), and another tracks filled slots (for consumers), ensuring proper coordination.
<!--ID: 1748186197897-->

Q: What is the potential drawback of using semaphores for all synchronization?
A: They can be error-prone and harder to reason about in complex systems compared to higher-level constructs like monitors.
<!--ID: 1748186197900-->

Q: Why might spinlocks be avoided in user-space programs?
A: Because they waste CPU cycles while waiting and are inefficient unless the critical section is extremely short or the contention is low.
<!--ID: 1748186197903-->


