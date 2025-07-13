### Interactive System Scheduling
- [[Fair-Share Scheduling]]
- [[Guaranteed Scheduling]]
- [[Lottery Scheduling]]
- [[Priority Scheduling]]
- [[Round-Robin Scheduling]]
- [[Shortest Job Next (SJN)]]
- [[Fundamentals/Operating Systems/Processes and Threads/Scheduling/Interactive System Scheduling/1_Overview]]

## Flashcards

TARGET DECK: Operating Systems::Scheduling::Interactive Scheduling

Q: What is the primary goal of interactive system scheduling compared to batch system scheduling?  
A: To ensure responsiveness and fairness for users or processes that require frequent CPU access, rather than maximizing throughput alone.
<!--ID: 1748186756629-->


Q: How does Round-Robin scheduling ensure fairness among processes?  
A: It gives each process a fixed time slice in cyclic order, ensuring that no process can monopolize the CPU.
<!--ID: 1748186756634-->


Q: What is a major drawback of basic Round-Robin scheduling in interactive systems?  
A: It doesn't account for process importance or workload differences, which can lead to inefficient CPU use if some processes require very short or long bursts.
<!--ID: 1748186756636-->


Q: How does Fair-Share Scheduling differ from traditional process-based scheduling?  
A: It allocates CPU time based on user or group ownership, ensuring each user gets a fair portion of CPU regardless of how many processes they run.
<!--ID: 1748186756639-->


Q: In what scenario does Fair-Share Scheduling outperform Round-Robin?  
A: When users have different numbers of processes, Fair-Share ensures equal CPU share per user, not per process, avoiding user-based starvation.
<!--ID: 1748186756642-->


Q: What is the central idea behind Guaranteed Scheduling?  
A: It ensures that each process gets at least a proportionate share of the CPU over time, based on its entitlement relative to others.
<!--ID: 1748186756645-->


Q: How does Guaranteed Scheduling measure fairness?  
A: It compares the actual CPU usage of a process to its expected share and prioritizes those falling behind their guaranteed allocation.
<!--ID: 1748186756648-->


Q: What is Lottery Scheduling, and how does it achieve fairness?  
A: A probabilistic method where each process holds tickets; the scheduler randomly selects a ticket each time, giving all processes a chance proportional to their ticket count.
<!--ID: 1748186756651-->


Q: How can Lottery Scheduling implement priority?  
A: By giving more tickets to higher-priority processes, increasing their chance of being selected.
<!--ID: 1748186756654-->


Q: What makes Lottery Scheduling flexible for resource allocation?  
A: It allows dynamic adjustment of ticket counts to reflect changing priorities, resource needs, or user policies.
<!--ID: 1748186756657-->


Q: How does Priority Scheduling decide which process runs next?  
A: It selects the process with the highest priority, either statically assigned or dynamically adjusted.
<!--ID: 1748186756659-->


Q: What is a major risk in Priority Scheduling, and how is it commonly addressed?  
A: Starvation of low-priority processes; mitigated using aging, where a process's priority increases the longer it waits.
<!--ID: 1748186756663-->


Q: What does Shortest Job Next (SJN) scheduling aim to optimize?  
A: It minimizes average waiting time by always selecting the process with the shortest expected execution time.
<!--ID: 1748186756666-->


Q: Why is SJN rarely used in practice despite its optimality?  
A: It requires exact knowledge or accurate prediction of future CPU burst times, which is often unavailable.
<!--ID: 1748186756669-->
