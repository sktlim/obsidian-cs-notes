### Real-Time System Scheduling
- [[Fundamentals/Operating Systems/Processes and Threads/Scheduling/Real-Time System Scheduling/1_Overview]]
- [[Real time systems]]


## Flashcards

TARGET DECK: Operating Systems::Scheduling:: Real time
Q: What distinguishes real-time scheduling from other types of scheduling?  
A: Real-time scheduling focuses on meeting strict timing constraints or deadlines, not just maximizing throughput or fairness.
<!--ID: 1748186834289-->


Q: What is the difference between hard and soft real-time systems?  
A: Hard real-time systems require all deadlines to be met without fail; missing a deadline is considered a system failure. Soft real-time systems tolerate occasional deadline misses but aim to minimize them.
<!--ID: 1748186834292-->


Q: What is Rate Monotonic Scheduling (RMS), and how does it assign priority?  
A: RMS is a fixed-priority scheduling algorithm where tasks with shorter periods (i.e., higher frequency) are given higher priority.
<!--ID: 1748186834294-->


Q: What are the assumptions behind Rate Monotonic Scheduling?  
A: Tasks are periodic, independent, have fixed computation time, and deadlines equal their periods.
<!--ID: 1748186834297-->


Q: What is the key limitation of RMS in real-time scheduling?  
A: It is only guaranteed to be schedulable if total CPU utilization is below a specific bound (e.g., ~69% for large task sets).
<!--ID: 1748186834301-->


Q: What is Earliest Deadline First (EDF) scheduling, and how does it prioritize tasks?  
A: EDF is a dynamic-priority algorithm that always schedules the task with the closest deadline.
<!--ID: 1748186834304-->


Q: How does EDF compare to RMS in terms of CPU utilization?  
A: EDF can achieve full CPU utilization (100%) in theory, making it more efficient than RMS under ideal conditions.
<!--ID: 1748186834306-->


Q: Why might EDF be harder to implement than RMS?  
A: EDF requires dynamic priority management and more complex tracking of deadlines at runtime.
<!--ID: 1748186834309-->


Q: What is the major risk of EDF in overload conditions?  
A: Under heavy load, EDF can degrade unpredictably, possibly causing multiple deadline misses, unlike RMS which fails more gracefully.
<!--ID: 1748186834312-->


Q: What is a periodic task in the context of real-time systems?  
A: A task that recurs at regular intervals with a defined period and deadline.
<!--ID: 1748186834315-->


Q: What is a sporadic task?  
A: A task that arrives irregularly but has a minimum inter-arrival time and a deadline for each occurrence.
<!--ID: 1748186834317-->


Q: What is a critical section, and why is it a challenge in real-time scheduling?  
A: A code region accessing shared resources; it can cause priority inversion, delaying higher-priority tasks if not properly managed.
<!--ID: 1748186834322-->


Q: What is priority inversion, and how is it resolved?  
A: When a high-priority task is blocked by a lower-priority one holding a needed resource. It’s resolved using priority inheritance, where the lower-priority task temporarily inherits the higher priority.
<!--ID: 1748186834326-->


Q: What is the primary goal of real-time scheduling?  
A: To ensure that all tasks complete within their timing constraints, especially in safety- or mission-critical applications.
<!--ID: 1748186834329-->


Q: What types of applications require hard real-time scheduling?  
A: Embedded systems in avionics, automotive systems, medical devices, and industrial control systems where timing failures can lead to critical errors.
<!--ID: 1748186834332-->
