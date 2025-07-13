### Process Creation
1) System Initialization
2) Execution of process-creation system call by running process: e.g. fork()
3) User request to create a new process
4) Initialization of batch job: mainly for batch systems where user submits batch jobs

### Process Termination
1) Normal exit (voluntary)
2) Error exit (voluntary): Exit gracefully
3) Fatal error (involuntary): illegal instruction, referencing non-existent memory, crash immediately
4) Killed by another process (involuntary)

### Process Hierarchy
- Unix processes belong to a single tree, with `init` at the root
	- Processes in Unix cannot disown their children
- Windows has no concept of process hierarchy
	- When process created, parent is given special token (called handle) that it can use to control child
	- handle can be passed to other processes, therefore invalidating any process hierarchy

### Process States
Possible state diagram might be
1. Running (actually using CPU)
2. Ready (runnable; temporarily stopped to let other process run)
3. Blocked (unable to run until some external event e.g. IO occurs)

![[Simplified State Diagram.png]]

### Process Implementation
- OS maintains a table called process table, with one entry per process (aka **process control blocks** (PCB))
- PCB contains everything that needs to be saved as a process transitions between process states 
#### Process Control Block 
| **Process Management**    | **Memory Management**         | **File Management** |
| ------------------------- | ----------------------------- | ------------------- |
| Registers                 | Pointer to text segment info  | Root directory      |
| Program counter           | Pointer to data segment info  | Working directory   |
| Program status word       | Pointer to stack segment info | File descriptors    |
| Stack pointer             |                               | User ID             |
| Process state             |                               | Group ID            |
| Priority                  |                               |                     |
| Scheduling parameters     |                               |                     |
| Process ID                |                               |                     |
| Parent process            |                               |                     |
| Process group             |                               |                     |
| Signals                   |                               |                     |
| Time when process started |                               |                     |
| CPU time used             |                               |                     |
| Children’s CPU time       |                               |                     |
| Time of next alarm        |                               |                     |

#### When interrupt occurs
1. **Hardware Stacks Program Counter and Other Critical Registers**
	- When interrupt occurs, the CPU stops current execution and saves the **program counter (PC)** and other critical registers (such as the **status register**) onto the **stack**.
	- The **program counter** contains the address of the next instruction to execute
	- Some systems also save other key registers like the accumulator or index registers, depending on the CPU architecture. This allows the CPU to preserve the current program's state before switching to handle the interrupt.

2. **Hardware Loads New Program Counter from Interrupt Vector**
	- The CPU then loads a new **program counter value** from a predefined location called the **interrupt vector table**.
	- The interrupt vector table is a specific memory region that stores addresses of interrupt service routines (ISRs) for each possible interrupt. Each interrupt type has an assigned address in this table.
	- By loading the new program counter value, the CPU jumps to the appropriate ISR, starting the execution of the code that will handle the interrupt
	
3. **Assembly Language (ASM) Procedure Saves Registers**
	- The ISR (often written in ASM for efficiency) begins by saving all the CPU registers (such as general-purpose registers and floating-point registers) onto the **stack** or another memory location.
	- This step ensures that the ISR can safely modify registers without affecting the state of the interrupted program.
	- Essential for ensuring **context integrity** so that, after the interrupt is handled, the CPU can restore the original state of the program and resume it seamlessly.
	
4. **Assembly Language Procedure Sets Up a New Stack**
	- The ISR typically switches to a **new stack** dedicated to handling the interrupt. This is particularly useful for systems that require isolation between user processes and the ISR, as the ISR needs its own stack space.
	- By setting up a new stack, the ISR avoids overwriting the stack of the interrupted program, which could lead to data corruption or unpredictable behavior.
	- The stack setup may involve loading a new **stack pointer** or setting a **stack frame** that the ISR will use for local variables and temporary storage.
	
5. **C Interrupt Service Routine (ISR) Runs (Reads and Buffers Input)**
	- Once the assembly setup is complete, control is handed over to a **C interrupt service routine** (or higher-level ISR code) that performs the actual interrupt handling logic.
	- This C ISR performs actions specific to the interrupt. For example, if the interrupt was triggered by a hardware device like a keyboard or network card, the ISR might **read input data** from the device and **buffer it** for later processing by the main program.
	- The ISR may also **acknowledge** the interrupt by sending a signal back to the hardware to indicate that the interrupt has been handled, allowing the device to resume normal operation.
	
6. **Scheduler Decides Next Program to Run**
	- After the ISR completes its main tasks, the **scheduler** (part of the operating system) is invoked to determine the next program or process to run.
	- The scheduler may decide to resume the interrupted process if there are no higher-priority tasks waiting, or it may select a different process if the system is multitasking or if a higher-priority process became ready during the interrupt.
	- The scheduling decision is based on factors like **priority levels**, **process states**, and **CPU time allotments**. If a different process is chosen, the system must perform a **context switch** to load that process's state.
	
7. **C Procedure Returns to Assembly Code**
	- After the C-level ISR completes and the scheduler has made its decision, control returns to the **assembly-level ISR**.
	- This C-to-assembly transition allows the low-level assembly code to handle the final steps of the interrupt handling process, such as restoring the saved CPU registers and preparing the CPU to resume execution of the selected process.
	- Returning to assembly gives the ISR more direct control over CPU registers, memory management, and stack handling, which are necessary to maintain system stability.
	
8. **Assembly Procedure Restores Registers and Starts Up New (or Original) Process**
	- The assembly ISR now **restores** all the previously saved CPU registers from the stack or memory, including the program counter, so the CPU can resume execution from where it left off.
	- If the scheduler decided to continue the interrupted program, the ISR restores the original program counter, effectively resuming the interrupted program.
	- If a context switch occurred, the program counter and other registers for the new process are loaded, and the CPU begins executing this new process instead.
	- After this final restoration, the interrupt handling is complete, and the CPU returns to normal operation, executing instructions for the next scheduled process.

### CPU Utilization
$$CPU \space Utilization = 1-p^n$$ where 
$p$ is fraction of time process spends waiting for IO to complete and 
$n$ is processes in memory at once

## Flashcards

TARGET DECK: Operating Systems::Processes

Q: What are the four main triggers for process creation in an operating system?  
A: System initialization, execution of a process-creation system call (e.g. fork), user request to create a new process, and the initialization of a batch job.
<!--ID: 1748185966411-->


Q: How does process termination differ between voluntary and involuntary scenarios?  
A: Voluntary termination includes normal exit and error exit (graceful), while involuntary termination includes fatal errors like illegal memory access or being killed by another process.
<!--ID: 1748185966414-->


Q: Why do Unix systems maintain a strict process hierarchy, and how is it different from Windows?  
A: Unix uses a single rooted tree structure with `init` as the root and does not allow disowning of child processes. Windows uses handles for parent-child interaction, which can be transferred, breaking strict hierarchy.
<!--ID: 1748185966416-->


Q: What are the three main process states in a simplified state diagram, and what differentiates them?  
A: Running (actively using the CPU), Ready (runnable but not executing), and Blocked (waiting on external events like I/O).
<!--ID: 1748185966419-->


Q: What is the role of the process control block (PCB) in process management?  
A: The PCB stores all information needed to track a process, including register contents, memory pointers, file descriptors, scheduling info, and identity data like process ID and user ID.
<!--ID: 1748185966422-->


Q: How does the operating system ensure that a process can resume correctly after an interrupt?  
A: It saves the program counter and critical registers, loads the appropriate ISR from the interrupt vector, saves all CPU registers in assembly, and restores everything after handling the interrupt or switching context.
<!--ID: 1748185966425-->


Q: Describe the role of the assembly-level ISR after a C interrupt handler completes.  
A: It restores saved registers, stack pointers, and the program counter, enabling the CPU to resume the interrupted process or switch to a new one based on scheduler decisions.
<!--ID: 1748185966427-->


Q: How does switching to a new stack during an interrupt help maintain process isolation?  
A: It prevents overwriting the interrupted process’s stack, avoids corruption, and ensures that the interrupt has its own isolated space for temporary data and local variables.
<!--ID: 1748185966430-->


Q: Why is the scheduler invoked after an interrupt service routine completes, and what decisions does it make?  
A: To determine whether to resume the interrupted process or switch to a higher-priority one, based on criteria like priority, time slices, and process states.
<!--ID: 1748185966433-->


Q: What is the function of the interrupt vector table during interrupt handling?  
A: It stores the addresses of all ISR entry points, allowing the CPU to jump to the appropriate handler when a specific interrupt occurs.
<!--ID: 1748185966435-->


Q: Why is it necessary for the C ISR to signal the hardware device after handling input?  
A: To acknowledge that the interrupt has been processed, allowing the device to continue operation and potentially raise new interrupts if needed.
<!--ID: 1748185966438-->


Q: Derive the formula for CPU utilization and explain what each variable represents.  
A: CPU Utilization = 1 − pⁿ, where **p** is the fraction of time a process waits for I/O and **n** is the number of processes in memory. It models the probability that at least one process is ready to use the CPU.
<!--ID: 1748185966441-->


Q: What happens if the value of p approaches 1 in the CPU utilization formula?  
A: CPU utilization drops significantly, indicating that processes are mostly waiting on I/O, leading to low overall CPU use.
<!--ID: 1748185966444-->


Q: Why does increasing the number of processes in memory (n) improve CPU utilization, according to the formula?  
A: It increases the likelihood that at least one process is ready to run while others are waiting on I/O, keeping the CPU busy more consistently.
<!--ID: 1748185966450-->


Q: What risk is introduced by an increasing number of processes in memory despite improved CPU utilization?  
A: Higher memory contention, increased context switching overhead, and possible thrashing if memory is overcommitted.
<!--ID: 1748185966454-->
