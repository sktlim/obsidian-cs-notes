### Batch System Scheduling
- [[Fundamentals/Operating Systems/Processes and Threads/Scheduling/Batch System Scheduling/1_Overview]]
- [[Advantages and Disadvantages]]
- [[First come first serve (FCFS)]]
- [[Shortest Job First (SJF)]]
- [[Shortest Remaining Time First (SRTF)]]


## Flashcards

TARGET DECK: Operating Systems::Scheduling::Batch Scheduling

Q: Why is Shortest Job First (SJF) considered optimal in terms of average waiting time, and under what condition does this optimality hold?  
A: SJF minimizes average waiting time because shorter jobs are completed first, reducing time spent waiting by subsequent jobs. This optimality holds only when exact job execution times are known in advance.
<!--ID: 1748183493639-->


Q: How does the lack of preemption in FCFS affect system responsiveness and overall throughput in batch scheduling?  
A: Without preemption, long-running jobs can block short ones, leading to poor responsiveness and lower throughput due to inefficient CPU use when short jobs wait unnecessarily.
<!--ID: 1748183493643-->


Q: What is the theoretical challenge in implementing Shortest Remaining Time First (SRTF) in a real operating system?  
A: SRTF requires real-time knowledge or prediction of the remaining execution time of every process, which is difficult to obtain or estimate accurately in most practical scenarios.
<!--ID: 1748183493646-->


Q: In a batch system, under what conditions could FCFS outperform SJF in terms of fairness or job starvation?  
A: FCFS may be preferable when job fairness is critical, as it prevents starvation by serving jobs strictly in arrival order, unlike SJF which may indefinitely delay long jobs if short ones keep arriving.
<!--ID: 1748183493649-->


Q: How does batch system scheduling trade off between CPU utilization and turnaround time, and which algorithms favor one over the other?  
A: Batch systems aim to maximize CPU utilization, often at the cost of individual turnaround time. FCFS favors utilization with simplicity, while SJF and SRTF aim to minimize turnaround and waiting time but may require more overhead and complex predictions.
<!--ID: 1748183493652-->


Q: What type of workloads are most suitable for SJF and why might it fail in a dynamic job arrival environment?  
A: SJF suits predictable, homogenous workloads with known job lengths. It struggles in dynamic environments where job lengths are unknown or jobs vary widely, leading to scheduling inaccuracies and potential unfairness.
<!--ID: 1748183493655-->


Q: Compare the preemption behavior of SRTF to Round Robin in the context of batch processing goals.  
A: SRTF preempts based on job length, optimizing turnaround time, whereas Round Robin preempts based on time slices, favoring fairness and responsiveness. In batch systems, SRTF better aligns with throughput and efficiency goals, while Round Robin may cause unnecessary context switches.
<!--ID: 1748183493658-->

Q: In the proof that Shortest Job First (SJF) minimizes average waiting time, what happens to the total waiting time if two adjacent processes with burst times B₁ and B₂ (where B₁ > B₂) are swapped so the shorter job runs first?
A: The total waiting time decreases because the longer job’s increased wait is offset by a greater decrease in the shorter job’s wait. Mathematically, swapping reduces total wait by B₁ - B₂.
<!--ID: 1748277368023-->

