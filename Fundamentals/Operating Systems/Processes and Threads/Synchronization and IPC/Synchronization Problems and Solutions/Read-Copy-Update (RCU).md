>Synchronization method that allows multiple readers to access data concurrently without locking, while writers update data by creating a modified copy and safely switching pointers after a grace period ensures no readers are using the old data

![[Read-copy-update.png]]
## Key Concepts in RCU
1. **Read-Copy-Update Mechanism**:
    - When a writer needs to modify a data structure, it creates a copy of the data structure (the **copy** phase), makes changes to the copy, and then **updates** a pointer to replace the old data structure with the modified version. (as seen in inserting nodes above)
    - This approach ensures that readers can continue to access the old version without interruption until they finish.
2. **Grace Periods**:
    - Readers access data structure through **read-side critical section**
    - RCU defines **grace periods** to ensure no readers are still accessing the old data before it is deleted.
	    - **Grace periods:** Any time period long enough to ensure that every reader has exited its read-side critical section at least once 
	    - As code in read-side critical section cannot block or sleep, simple solution is waiting till all threads have executed a context switch
    - After a writer updates the pointer to the new version, RCU waits until all current readers have completed before freeing or deleting the old data. This guarantees that no reader ever accesses deleted or outdated data.
3. **Deferred Freeing**:
    - RCU uses deferred freeing, which is a way to safely release memory after the grace period ends.
    - By postponing the deallocation until no readers are accessing the old data, RCU avoids potential crashes or inconsistent reads, thus ensuring safe memory management.

## Advantages of RCU
- **High Read Performance**: RCU allows readers to access data without locks, minimizing contention and improving performance for read-heavy workloads.
- **Efficient for Read-Mostly Scenarios**: RCU is particularly well-suited for systems where read operations are much more frequent than writes, as it eliminates the need for locking mechanisms on reads.
- Quite popular in OS kernels for data structures. Linux kernel has many uses for its RCU API

## Limitations of RCU
- **Complexity in Writes**: Writing with RCU is more complex because it requires making copies, updating pointers atomically, and managing grace periods, which can increase implementation complexity.
- **Limited Use Cases**: RCU is not suitable for scenarios with frequent writes, as its efficiency primarily benefits read-heavy environments.