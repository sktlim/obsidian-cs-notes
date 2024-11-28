## Definitions

**Concurrency:** 2 or more separate activities happening at the same time; Single system performing multiple independent activities in parallel; When primary concern is separation of concerns or responsiveness

**Parallelism:** When primary concern is taking advantage of available hardware to increase performance of bulk data processing

**Task Switching:** Switching between tasks so fast they seem like they are being done concurrently

**Hardware Concurrency:** Hardware genuinely supporting concurrency through multi

**Context switch:** Change from one task to another. OS saves CPU state and instruction pointer for current running task, work out next task to run, and reloads CPU state for task being switched to

**Thread:** Smallest sequence of programmed instructions that can be managed independently by a scheduler; Lightweight execution context that contains values of CPU registers needed to execute instructions

**Process:** Instance of a computer program being executed by a thread. Execution of program instructions after loading from disk to memory

**Task Parallelism:** Divide single task into multiple and run in parallel [^1]

**Data Parallelism:** Each thread performs same operation on different parts of data[^1].   

[^1]: Tasks that are susceptible to these task/data parallelism are known as embarrassingly parallel/ naturally parallel/ conveniently concurrent

### Threads

Threads in a process share:
- Address space