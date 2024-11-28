This overview focuses on the programmer's main model of memory in OS.
## Models
### No Memory Abstraction
- Earliest models interfaced directly with registers and memory addresses
- On this model, only 1 process can run at a time
- To get some parallelism, can use threads since threads are designed with the intention of sharing the same address space, but this idea is of limited use
#### Running multiple programs with no memory abstraction
- **Swapping:** To run multiple programs, OS can save memory to a file on non-volatile storage, then context switch and run another program
- Issue with this method is that memory addresses in programs refer to absolute addresses, which can easily cause a crash (especially on IBM 360 according to Tannebaum textbook)
- Stop-gap solution: **Static Relocation**
	- constant offset of where program started added to absolute memory addresses
		- If program loaded at address 16,384, constant 16,384 added to every program address during load
	- Not very general solution and slows down loading

- What we want is programs that can reference their own private/ relative memory address, also so that they can't destroy our OS
### Memory Abstractions
- Basic way: [[Address Spaces]]
- Virtual Memory
