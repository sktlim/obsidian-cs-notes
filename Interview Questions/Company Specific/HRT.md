## 2023
Can you describe a timeline about how this framework might go from running task A to B? What states do you need to store? How does that state change when switching between tasks? abstract concurrent framework callable task submitted by clients multiple task concurrently by multiple clients non-preemptive process ended or switch() called

readswitch(fileDescriptor) will allow framework to run other tasks. poll(openDescriptors, timeout) returns a list of ready file descriptors. Timeout is for controlling when timeout returns without any openDescriptors. How will you implement readSwitch? what if all the tasks are waiting for io? would you need to something to store the task waiting on io?

You're in charge of NY contact tracing effort `dailyRecord` class going to contain a set of citizens that tested positive, second is `unordered_set` of meetings (set of pairs) between citizens. Implement these classes to determine all infected citizens at end of the day

if given time series data where ith daily record contains details for the ith day. Find number of possible infected at end of the data

Suppose that you have a trading system that trades on a stream of events Trade on US equity stock info For each symbol, it will have current price info and other reference data it has loaded at start of process Send a buy/sell order at appropriate size and price Latency is currently very inconsistent How to troubleshoot?