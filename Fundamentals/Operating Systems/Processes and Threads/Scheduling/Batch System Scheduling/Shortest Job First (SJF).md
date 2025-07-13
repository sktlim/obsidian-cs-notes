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

## Proof that SJF minimizes average waiting time
### Problem Setup
Assume we have $n$ processes.  
Each process $P_i$ has a burst time $B_i$.  
All processes arrive at time 0.  
We are using non-preemptive scheduling.
### Goal
Show that scheduling processes in ascending order of burst time (Shortest Job First) minimizes average waiting time.
### Definitions
- Waiting time of process $P_i$:  
  $$ W_i = \text{Start time of } P_i - \text{Arrival time of } P_i $$
  Since all arrive at time 0, $W_i$ is just the start time.
- Average waiting time:  
  $$
  \text{Average} = \frac{1}{n} \sum_{i=1}^{n} W_i
  $$
### Proof (Using Exchange Argument)
Assume an arbitrary schedule $S$ that is not sorted by burst time.  
Then, there exists at least one pair of adjacent processes $P_i$ and $P_{i+1}$ such that:
$$
B_i > B_{i+1}
$$
#### Step 1: Swap the two processes
Let $T$ be the total time that has elapsed before $P_i$ starts.
- Original waiting times:
$$ 
\begin{aligned}
W_i &= T  \\
W_{i+1} &= T + B_i 
\end{aligned}
$$
- After the swap:
$$
\begin{aligned}
W_{i+1}' &= T \\
W_i' &= T + B_{i+1} 
\end{aligned}
$$
#### Step 2: Compare total waiting times
Original total:
$$
W_i + W_{i+1} = T + (T + B_i) = 2T + B_i
$$
After swap:
$$
W_i' + W_{i+1}' = (T + B_{i+1}) + T = 2T + B_{i+1}
$$
Since:
$$
B_i > B_{i+1}
$$
It follows that:
$$
2T + B_i > 2T + B_{i+1}
$$
Thus, swapping reduces total waiting time.

#### Step 3: Repeat swaps to reach SJF order
By repeatedly swapping such pairs, we can reach the SJF order.  
At each step, the total waiting time decreases or stays the same.

### Conclusion
The SJF (Shortest Job First) schedule minimizes total and therefore average waiting time among all non-preemptive schedules when all processes arrive at time 0.
