> Synchronization primitive used in concurrent programming to ensure that multiple threads or processes reach a certain point of execution before any of them proceed further

- When process reaches barrier, it blocks until all processes reach barrier
- Meant for groups of processes

### Example usage
- Calculating a transformation to some large matrix
	- Different processes can work on different parts, but all processes have to wait for all others to finish their current work

## Related theory: Memory barriers/ fences
- Enforce an order to guarantee that all memory operations started before the barrier instruction will also finish before all the memory instructions issued after the barrier
- Mitigates CPU vulnerabilities known as [[Transient Execution Vulnerabilities]]