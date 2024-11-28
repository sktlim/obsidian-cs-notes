> New ready processes scheduled enter queue of all ready processes and are served in order

## Properties
- Non-preemptive
## Advantages
- **Simple and Easy to Implement**: FCFS is straightforward, as it processes jobs in the order they arrive without requiring complex logic.
- **Fairness**: Every job gets a chance to execute in the order it was received, preventing starvation of any job.
## Disadvantages
- **Poor Average Waiting Time**: Long jobs can delay shorter jobs behind them, resulting in the **convoy effect** where short jobs wait for longer ones to complete.
- **Not Ideal for Time-Sensitive Tasks**: Since it doesn’t prioritize urgent jobs, it’s not suitable for systems requiring quick response times.