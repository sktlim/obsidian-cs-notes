## Common questions
[[Difference Between a Mutexes, Semaphores and Condition Variables]]
### Key Idea
- **Semaphore as a Counter**:
    - A semaphore is an integer variable that acts as a counter for managing concurrent access to shared resources.
    - The counter represents the number of "permits" available for processes, effectively tracking how many processes can proceed or how many "wakeups" are saved for future use.
- **Atomic Operations**:
    - Semaphores rely on two atomic operations, `down` (or `wait`) and `up` (or `signal`), to avoid race conditions and ensure mutual exclusion when modifying the semaphore's value.

### Details of `down` and `up` Operations

1. **`down` (or `wait`) Operation**:
    - This operation checks if the semaphore value is greater than 0.
	    - If `semaphore > 0`, it decrements the semaphore (`semaphore--`) and allows the process to continue.
	    - If `semaphore == 0`, the process cannot proceed, so it "sleeps" or is blocked and added to a queue of waiting processes until the semaphore becomes available (when another process performs `up`).
```cpp
down(semaphore) {
    if (semaphore > 0) {
        semaphore--;  // Decrement and continue if resources are available
    } else {
        cur_process.block;  // No resources available
        process_queue.push_back(cur_process);
    }
}
```
1. **`up` (or `signal`) Operation**:
    - This operation increments the semaphore, effectively signaling that a resource has been released.
    - If one or more processes are waiting (blocked on the `down` operation), the `up` operation will "wake up" one of these processes, allowing it to complete its `down` operation and proceed.
    - Importantly, if there are waiting processes, the semaphore stays at 0 since only the waiting process continues; otherwise, the semaphore is incremented.
```cpp
up(semaphore) {
    if (!process_queue.empty()) {
        wake one waiting process;  // Allow one blocked process to proceed
    } else {
        semaphore++;  // Increment to make resource available for future `down` calls
    }
}
```
## Usages
- Used to solve [[Producer-Consumer Problem]]
## Flashcards

TARGET DECK: Operating Systems::Processes and Threads::Synchronization Primitives

Q: How does a semaphore work?
A: It has an integer variable acting as a counter to count how many threads has access to shared resources. The `up`/`signal` operation increments the semaphore, signaling that resource has been released. The `down`/`wait` operation checks if the semaphore is > 0. If yes, it decrements and allows process to continue, but if semaphore == 0, it blocks the process and queues it.
<!--ID: 1748181369665-->
