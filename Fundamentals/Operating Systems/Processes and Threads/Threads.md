> Smallest sequence of programmed instructions that can be managed independently by a scheduler

### Properties
- In many cases, thread is a component of [[Processes]]
- Multiple threads can be executed concurrently, sharing resources such as memory
- Threads of a process share its <u>executable code</u> and the <u>values of its dynamically allocated variables</u> and <u>non-thread-local global variables</u> at any given time
- There is a chance for **spurious wakeups** to happen because in between the time when the condition variable was signaled and when the awakened thread was finally able to run, another thread ran first and changed the condition again
	- That's why its recommended to use `while()` instead of `if()` to check condition variables 

### Classical Thread Model

| Per-process Items       | Per-Thread items |
| ----------------------- | ---------------- |
| Address space           | Program Counter  |
| Global variables        | Registers        |
| Open files              | Stack            |
| Childs processes        | State            |
| Pending alarms          |                  |
| Signal/ signal handlers |                  |
| Other info              |                  |

### Linux implementation
- `fork()` of multithreaded process will only create a single thread in the child
	- However, using POSIX threads, a program can use `pthread_atfork()` to register fork handlers (procedures called when fork occurs) to start additional threads

### POSIX Threads
| **Thread call**        | **Description**                                      |
| ---------------------- | ---------------------------------------------------- |
| `pthread_create`       | Create a new thread                                  |
| `pthread_exit`         | Terminate the calling thread                         |
| `pthread_join`         | Wait for a specific thread to exit                   |
| `pthread_yield`        | Release the CPU to let another thread run            |
| `pthread_attr_init`    | Create and initialize a thread’s attribute structure |
| `pthread_attr_destroy` | Remove a thread’s attribute structure                |
### User space thread vs kernel thread 
![[User space thread vs kernel thread.png]]

- User space 
	- If thread is in user space, each process needs its own **thread table** in the runtime system to keep track of thread-specific information like the thread's program counter, registers, stack etc. This enables it to do thread switching quickly (especially if the runtime system provides instructions to store/ load all registers)
	- Pros
		- Thread switching is faster in this manner than trapping to the kernel and is an advantage for user space threads
		- User space threads also allow for each process to control their thread scheduling algorithm
	- Cons
		- Implementing blocking system calls are an issue as one thread making a system call would block all other threads
			- This is resolved in a roundabout way using `select()` to first check if a subsequent system call will be blocking and only allowing the call if `select()` deems that it will not block
			- If `select()` decides that a `read()` will block, it will yield to another thread and not make the call
		- Another problem is first thread must voluntarily `yield()`, otherwise other threads will not get CPU
		- Another problem is programmers want threads in places where threads block often e.g. web server 
- Kernel
	- Kernel has thread table tracking all threads in the system
	- When thread blocks, kernel can choose among all threads of all processes to determine next one to run
	- Calls that might block are implemented as system calls, making it more expensive
	- Pros
		- Avoids all the issues with difficult implementation
	- Cons
		- If threads call system calls often, its expensive
		- If multithreaded process `forks()`, replicating all threads might be ideal
			- E.g. web server process with 3 threads doing different things. If process forks, replicating the 3 threads might be necessary to make sure that child process has same functionality as original
		- If multiple threads are looking out for a signal, choosing (or allowing it to be random) the thread that handles it is also a concern
- Hybrid Implementation
	- One model is to use kernel-level threads and multiplex user-level threads onto some/ all of them. This model gives best flexibility![[Hybrid threading model.png]]
	- Kernel is only aware of kernel threads, and can schedule those appropriately

### Multithreading issues
- Global variables accessible by all threads are an issue
	- Possible solution: make private copy of global variable for all threads
- Library procedures not reentrant i.e. not designed to have second call made when first call hasn't finished
	- Using mutex with library functions potentially eliminates a lot of parallelism
- Some signals are thread specific (`alarm()`), while others are not (keyboard interrupt)
- Kernel does not have access to thread's stack; if thread stack overflows, it cannot grow them automatically upon stack fault