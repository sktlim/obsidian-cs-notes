>**Shared Libraries**, known as **Dynamic Link Libraries (DLL)** on Windows, are external files containing **compiled code and data** that can be loaded onto memory and used by multiple programs at runtime

## Traditional (Static) Linking
- Object files/ libraries are named in the command to the linker
- Functions called in object file but not present are known as **undefined externals** and linker will try to look for them and include them
- When linker is done, executable binary file is written to non-volatile storage containing all functions needed

- Leads to space wastage in non-volatile storage as some libraries that are commonly used can be shared

## Dynamic Linking/ Shared Libraries
- Linker includes a small stub routine that binds to called functions at runtime 
- Shared libraries either loaded when program is loaded or when shared library is loaded for first time (depending on config)
- Library paged in as needed
- Makes executable smaller, and programs that call functions in shared libraries need not recompile if shared function is updated

- However, if shared library uses absolute address, if Process A (at memory location 32K) and Process B (at memory location 16K) call shared library function which jumps to address 16 in the library, there is a mismatch between addresses
- **Solution:** Use compiler flags to instruct compiler not to produce instructions using absolute addresses, and only relative offsets. This is known as **position-independent code**
- In Linux, `gcc -fPIC -c mathlib.c -o mathlib.o` generates such code with option `-fPIC` being that flag

## Memory Mapped Files
- Shared Libraries are special case of **memory-mapped files**
- Idea is to map a file in non-volatile storage onto some part of a process' virtual address space, then COW it
- If processes map onto the same file, they communicate using shared memory