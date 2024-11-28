1) **Taking advantage of parallelism**
	1) Spread data across many disks for parallel reads $\rightarrow$ data-level parallelism
	2) Pipelining 
		1) Overlap instruction execution to reduce total time to complete instruction sequence
		2) **Key insight**: Not every instruction depends on its immediate predecessor, so executing instructions in parallel is possible 
	3) Detailed digital design
		1) Modern ALUs use [[Carry-lookahead]] to speed up summing many number
2) **Principle of locality**
	1) **Temporal locality:** Programs tend to reuse data and instructions that have been used recently 
	2) **Spatial locality:** Items whose addresses are near one another tend to be referenced close together in time 
	3) Rule of thumb: program spends 90% of execution time in 10% of code
3) **Focus on the common case**
	1) Frequent case is often simpler than the infrequent case