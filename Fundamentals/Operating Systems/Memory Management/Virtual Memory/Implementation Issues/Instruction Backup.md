>Reverting to the instruction that caused a page fault after OS fetches the page is hard to implement

## Problem Example
- Sample instruction in Motorola 680x0: `MOV 6, 2` 
- ![[Instruction Backup Example.png]]
- Depending on which memory reference caused the page fault, PC can be 1000, 1002, or 1004 at time of fault
- OS has no way of telling whether the word in 1002 is a memory address associated with instruction at 1000 or an opcode

- On some machines, designers leave a hidden internal register into which PC is copied just before each instruction is executed
- Machines may also have 2nd register telling which registers have been autoincremented and by how much
- With this, OS can unambiguously undo the instruction 

- If info not available, OS has to jump through hoops to solve this issue