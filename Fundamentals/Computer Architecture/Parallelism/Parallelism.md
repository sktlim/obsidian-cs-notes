### Base Parallelisms
[[Data-level Parallelism]]: Many data items can be operated on at the same time

[[Task-level Parallelism]]: Tasks are created that can be operated independently and largely in parallel

### Higher level Parallelisms
[[Instruction-Level Parallelism]]: Exploits data-level parallelism at modest levels with compiler help using ideas like pipelining and at medium levels using ideas like speculative execution

[[Vector Architectures and GPUs]]: Exploits data-level parallelism by applying a single instruction to a collection of data in parallel 

[[Thread-level Parallelism]]: Exploits either data-level parallelism or task-level parallelism in a tightly coupled hardware model that allows for interaction among threads 

[[Request-level Parallelism]]: Exploits parallelism among largely decoupled tasks specified by programmer or OS

## Categories of multiprocessors
- **SISD: Single instruction stream, single data stream** 
	- uniprocessor. Standard sequential computer, but can exploit [[Instruction-Level Parallelism]] 
- **SIMD: Single instruction stream, multiple data stream** 
	- Same instruction, multiple processors using different data streams
	- Exploit [[Data-level Parallelism]] by applying same operation to multiple items of data in parallel
- **MISD: Multiple instruction stream, single data stream**
	- No commercial multiprocessor of this type to date
- **MIMD: Multiple instruction stream, single data stream**
	- Each processor fetches its own instructions and operates on its own data, and it targets task-level parallelism
	- More flexible than SIMD, but more expensive