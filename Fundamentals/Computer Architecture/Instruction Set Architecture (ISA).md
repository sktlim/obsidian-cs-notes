## 7 dimensions of ISA
1) Class of ISA
	1) Register-memory ISAs e.g. 80x86; can access memory as part of many instructions
	2) load-store ISAs e.g. ARM and MIPS; can access memory only with `load` or `store` instructions
	3) All recent ISAs are load-store
2) Memory addressing
	1) Virtually all use byte addressing to access memory operands
	2) Some architectures (ARM and MIPS) require objects to be aligned
	3) 80x86 does not require this, but would generally be faster if they are
3) Addressing modes
	1) MIPS addressing mode is `Register`, `Immediate` and `Displacement` where constant offset is added to register to form the memory address
	2) 80x86 supports these 3 + 3 variations of displacement. See book page 12 for more info
4) Types and sizes of operands
	1) Most ISAs support 8-bit (ASCII), 16-bit (unicode), 32-bit (integer or word), 64-bit (double word or long integer) and IEEE 754 floating point in 32-bit and 64-bit 
	2) 80x86 also supports 80-bit floating point (extended double precision)
5) Operations
	1) Data transfer, arithmetic, logical
6) Control flow instructions
	1) Virtually all ISAs support conditional branches, unconditional jumps, procedure calls and returns
	2) 80x86, ARM and MIPS use PC-relative addressing (branch address specified by address field that is added to PC)
7) Encoding an ISA
	1) Two basic choices: fixed length and variable length
	2) ARM and MIPS are 32-bits long 
	3) 80x86 encoding is variable length, ranging from 1-18 bytes
	4) Variable can take less space than fixed 