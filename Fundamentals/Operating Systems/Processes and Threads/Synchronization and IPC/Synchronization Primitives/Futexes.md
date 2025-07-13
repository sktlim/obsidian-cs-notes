> Fast user space mutex. User space is preferred to avoid syscall overheads so futex does this by using user space as much as possible and entering kernel only when necessary

- **Lock contention:** Multiple threads trying to acquire the lock at the same time
- **Linux** feature that allows threads to avoid syscalls when there is no contention, and only involve kernel in cases where threads are forced to wait; Low level enough that it probably won't be used by most users
## How it works
- **Fast Path (User Space)**:
    - When a thread tries to acquire a futex, it first checks the futex’s integer value in **user space**. If the `futex == 0` meaning its free, it simply changes this value (e.g., to 1 to indicate the lock is taken) and proceeds.
    - Since this operation happens entirely in user space, it is fast and does not require a system call or kernel intervention.
    - Similarly, when a thread releases the lock, it can reset the futex’s value in user space, allowing other threads to acquire the lock without kernel involvement.

- **Slow Path (Kernel Intervention)**:
    - If a thread finds that the futex is already locked (the integer value indicates it’s taken), it goes into the **slow path** and makes a system call to the kernel to avoid busy-waiting.
    - The thread calls `futex()` with the **FUTEX_WAIT** operation, which tells the kernel to put the thread to sleep until the futex value changes, indicating the lock might be available.
    - When another thread releases the lock, if it finds that other threads are waiting (indicated by checking the futex’s value again), it performs a **FUTEX_WAKE** operation, which wakes up the waiting threads so they can attempt to acquire the lock again.

## Properties
- 32 bit value even on 64 bit systems
## Operation
- **FUTEX_WAIT**:
    - A thread uses `FUTEX_WAIT` to tell the kernel to put it to sleep if the futex is unavailable. The kernel monitors the futex’s memory address, and the thread only sleeps if the futex value hasn’t changed (avoiding a race condition).
    - This operation helps avoid busy-waiting and CPU usage for threads waiting on a lock.
- **FUTEX_WAKE**:
    - When a thread releases a lock, it calls `FUTEX_WAKE` to wake up one or more threads waiting on that futex.
    - The kernel then transitions the waiting threads from a sleep state to runnable, allowing them to attempt to acquire the lock again in user space.