> Assumes runtimes are known in advance. When several of these equally important jobs are in process ready queue, scheduler serves the shortest job first

## Properties
- Provably optimal only when all jobs are simultaneously available. If there are new jobs that haven't arrive, optimality is not guaranteed
- Non-preemptive
## Advantages
- **Optimal Average Waiting Time**: By prioritizing shorter jobs, SJF minimizes the average waiting time, making it efficient in scenarios where short tasks are frequent.
- **High Throughput**: SJF processes many short jobs quickly, leading to high throughput and efficient resource usage.
## Disadvantages
- **Risk of Starvation**: Long jobs may wait indefinitely if new short jobs keep arriving, leading to starvation.
- **Difficulty in Predicting Job Length and limited applicability**: SJF requires knowledge of each job's length, which is challenging to predict in real-time systems and more applicable only in batch systems