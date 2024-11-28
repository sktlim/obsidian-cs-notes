### Problem with IPC: Race conditions
- When 2 or more processes are reading/ writing to shared data and final results depends on who runs when
#### Example: Print spooler
- When process wants to print a file, it enters file name in a **spooler directory**
- **Printer daemon** periodically checks whether there are new file names in this directory, and if so, starts a print job and removes the file names from this directory
- Process A and B decide that they want to queue a file for printing
	- Slot _x_ is free and A reads shared variable `in` (storing next free slot) and gets _x_
	- CPU context switches to B and B reads shared variable `in`  and gets _x_ 
	- Both processes think they are storing their file in slot _x_, and further reads/writes are potentially inconsistent with A or B's expected logic

### Solution: Critical Regions
- We need **mutual exclusion**, some way to ensure that if one process is using a variable or a file, other processes cannot use it
- **Critical Region:** Part of program where shared memory is accessed 

##### <u>Conditions for avoiding race conditions</u>
1) **Mutual Exclusion**: No 2 processes can simultaneously be inside critical regions
2) No assumptions may be made on speed or number of CPUs
3) **No unnecessary blocking:** No process running outside critical region may block any process
4) **Bounded Waiting:** No process should wait forever to go into critical region
#### Attempt 1: Disabling interrupts ☒ 
- On single processor system, simplest solution is to **disable interrupts when entering critical region** and reenable them before leaving it
	- No clock interrupts can occur and CPU will not switch to another process
- **Pros**
	- Frequently convenient for kernel itself to disable interrupts for a few instructions
- **Cons**
	- What if process disables and never reenables interrupts?
	- Interrupts on 1 CPU is disabled, but other CPUs are not affected and can still access shared memory
- **Conclusion:** Useful for OS itself but not appropriate for general mutex mechanism for user processes as it violates condition 2 and possibly 4. Kernel should not disable itself for more than a few instructions less it misses interrupts

#### Attempt 2: Lock Variables ☒
- Have a single (shared) **lock variable** initially set at 0
	- When process wants to enter critical region, set it to 1 and enter
	- When process wants to leave critical region, set it to 0 and exit
	- If lock already 1, process waits for variable to become 0 
- **Conclusion:** Inadequate as context switch can happen after process checks lock variable and before it sets the variable and enters critical region, violating condition 1

#### Attempt 3: Strict Alternation ☒
![[Proposed busy waiting solution.png]]
- **Busy waiting:** Have process continuously test variable until value appears
- **Spin lock:** lock that uses busy waiting
- Should be avoided as it wastes CPU time
- Violates condition 3 above
	- If `turn = 1` (assume process A and B finishes one iteration and are both at `noncritical_region()`), and process B looks back to the first line, B is blocked by A which is not in it's critical region
- **Conclusion:** Race condition avoided but violates condition 3 so no

#### Attempt 4: Peterson's solution ☑

```cpp
int turn;
int interested[N];

void enter_region(int process){
	int other; // number of other processes

	other = 1 - process;
	interested[process] = true;
	turn = process; // set flag
	while (turn==process && interested[other]==true);
}

void leave_region(int process){
	interested[process] = false;
}
```

- Code above is for two processes only and is a type of **spinlock**. For general solution, see [[Peterson's Solution for Race Conditions]]
- Before using shared variables, each process calls `enter_region(pid)`, which is a function that causes process to wait if needed until it is safe to enter
- After it finishes, process calls `leave_region(pid)` to indicate that it is done

- If process 0 call `enter_region()`, then process 1 tries to call, it will busy wait in the while loop as `interested[0] = true`
- If both try to call `enter_region()` simultaneously, whoever stores their process number in `turn` is the one that waits. That is:
	- process 1 stores `turn = 1` last and both process 0 and process 1 hit the while loop
	- process 0 does not execute while loop as `turn(1) != process(0)` and enters critical region
	- process 1 waits

**Conclusion:** This works and does not violate the conditions for a good solution. However, utilizes busy waiting

#### Another solution: Test and Set Lock (TSL) Instruction ☑
- Some computers have the TSL (Test and Set Lock) instruction
```asm
TSL RX,LOCK
```
- Instruction reads contents of the memory word `LOCK` into register `RX` and stores a nonzero value at the address of `LOCK`
	- If `RX` is zero, the process proceeds into the critical section and sets value of `LOCK` to indicate it is taken
	- If `RX` is not zero, the process knows the lock is already held by another process and may either busy-wait or perform some other action to try again.
- Operations of reading and storing the word are guaranteed to be indivisible (no other process can access the word until instruction is finished)
	- To guarantee this, CPU executing instruction locks memory bus to prohibit other CPU from accessing
	- Different from disabling interrupts as disabling interrupts has no effect on keeping another processor from accessing memory to change the shared memory word
- To use TSL, a shared variable `LOCK` is used for coordination
	- When `LOCK == 0`, any process can set it to 1 using TSL and then r/w shared memory
	- When process is done, lock is set back to 0 using `MOV LOCK, 0`

##### Pseudocode solution
```asm
LOCK = 0  ; Shared memory location for the lock, initialized to 0 (unlocked)

acquire_lock:
    TSL RX, LOCK_VARIABLE  ; Atomically load LOCK into RX and set LOCK to 1
    CMP RX, 0              ; Check if RX is 0 (was unlocked)
    JNE acquire_lock       ; If RX is not zero, another process holds the lock; retry
    ; Critical section code goes here

release_lock:
    MOV LOCK, 0   ; Release the lock by setting it to 0
```

#### Alternative to TSL: XCHG
- `XCHG` exchanges contents of 2 locations atomically e.g. memory word and register

```asm
enter region:
	MOVE REGISTER,1          | put a 1 in the register (if locked, stays locked)
	XCHG REGISTER,LOCK       | swap the contents of the register and lock variable
	CMP REGISTER,0           | was lock zero?
	JNE enter region         | if it was non zero, lock was set, so loop
	RET                      | retur n to caller; critical region entered
	
leave region:
	MOVE LOCK,0              | store a 0 in lock
	RET                      | retur n to caller
```

TSL and XCHG both use busy waiting which is not ideal

