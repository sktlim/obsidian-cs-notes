## File Descriptor `fd`
- positive integer number used by file system calls instead of a file path in order to make a variety of operations
- Each process has its own **file descriptor table**
- Main idea is to decouple a file path (or more accurately, an inode with minor and major device numbers) from a file object inside a process and the Linux kernel
- Allows devs to open same file many times for different purposes with various flags (e.g. `O_DIRECT`, `O_SYNC`, `O_APPEND`) and at different offsets
- Example:
	- Program wants to read from and write to one file in two separate places, so it needs to open file twice
	- Two new file descriptors will refer to 2 different entries in system-wide **open file description table**

## Open File Description Table
- system-wide kernel abstraction storing file open status flags
- Stores file open status flags and file positions

- Every created process in Linux kernel has per-thread struct `task_struct`, which has a pointer to `files_struct`, that contains an array of pointers to `file` struct
- `file` struct is what holds file flags, current position and other info about the open file such as type, inode, device
- All such entries among all running threads refers to the open file descriptor table

- To create new entry in open file description table, we need to open a file with one of the following syscalls: `open`, `openat`, `create`, `open2`
- These functions add a corresponding entry in the file descriptor table of the calling process, build a reference between the open file description table entry and the file descriptor table, and return the **lowest positive number not currently opened by the calling process**
- Last statement important because it means `fd` number can be reused during process life if it opens and closes files in an arbitrary order

- Linux kernel also provides API to create copies of a file descriptor within a process using `dup`, `dup2`, `dup3` and `fcntl` with `F_DUPFD` flag
- All these syscalls create new reference in `fd` table of process to existing entry in the system-wide open file description table

![[Linux file descriptors.png]]
1) `fd` 1,2,3 (`stdin`, `stdout`, `stderr`) are special `fd`s. All 3 point to pseudoterminal (`/dev/pts/0`). Files don't have positions due to their character device type. process_1 and process_2 must hence be running under terminal sessions. `stdout` of process_2 points to file on disk `/tmp/out.log` (shell redirection)
2) Some `fd`s can have per-process flags. Nowadays there is only the close-on-exec `O_CLOEXEC` flag. Some `fd`s may have it for the same system-wide open file description table entries, e.g. process_1 and `fd` 9, process 2 and `fd` 3
3) There might be gaps between `fd` numbers e.g. `fd` 9 after `fd` 3 in process_1 as `fd` 4,5,6,7 may already be closed. Another way could be explicit duplication of `fd`. Using these syscalls, we can specify the desired file descriptor number to duplicate
4) Process can have more than 1 `fd` that points to same entry in open file descriptions through duplication syscalls. `fd 0` and `fd 2` of process_2 refer to same pseudo terminal entry
5) Sometimes, standard `fd`s might be pointed to a regular file (or pipe) and not a terminal, e.g. `stdout` of process_2 refers to file on disk `/tmp/out.log`
6) Possible to point `fd`s from various processes to same entry in open file description table by a `fork` call and inheriting `fd`s from parent to child. Descriptors could also have different integer `fd` numbers inside processes and different process flags (`O_CLOEXEC`) e.g. `fd9` of process_1 and `fd 3` of process_2
7) Linux kernel typically uses inode numbers, minor and major numbers of a device instead of a string path
8) Often for a shell, 0, 1, and 2 `fd`s are pointed to a pseudoterminal
9) Multiple open `fd` entries can be linked with same file on disk

### `stdin`, `stdout`, `stderr`
- First 3 `fd`s of processes have well-known aliases `stdin`, `stdout`, `stderr` in that order
- For process started and running within terminal session, these `fd`s can be pointed to a pseudoterminal, a terminal, a file, a pipe.... For classical UNIX style daemon, they usually refer to a /dev/null device

### Procfs and file descriptors
- Kernel exposes all open file descriptors of process with virtual `procfs` file system
- To get info, we can use shell variable `$$` with PID e.g. `$ ls -l /proc/$$/fd/`
- Another useful directory in `procfs` is `fdinfo` folder under process directory containing per file descriptor info e.g. `stdin` of current shell: `$ cat /proc/$$/fdinfo/0`
- Flags section here contains only status flags

### Sharing `fd` between parent and child after `fork()`
- After `fork()` or `clone()` (without `CLONE_FILES` set) call, a child and parent have an equal set of `fd`s, which refer to the same entries in the system-wide open file description table, meaning that they share identical file positions, status flags and process `fd` flags (`O_CLOEXEC`)
- Primary purpose of sharing is to protect files from being overwritten by both children and parent process. If all relatives start writing to file simultaneously, kernel will sort this out and not lose data 
- Data might appear in a mixed way; if it's not intended, file descriptors should be closed after a successful `fork()` (including 3 standard ones). This is how classical daemons start

### Duplication of file descriptors
- `dup()`: creates new `fd` using lowest unused int number. Usually follows `close()` syscall for one of the standard `fd` (`stdin`, `stdout`, `stderr`) in order to replace it
- `dup2()`: does same as `dup()` but has 2nd argument to specify target `fd`. If target `fd` already exists, `dup2()` closes it first. All `dup2()` operations are atomic
- `dup3()`: same as `dup2()` but with 3rd argument to set `O_CLOEXEC` flag
- `fcntl()` with `F_DUPFD` flag behaves as `dup2()` unless target `fd` exists, in which case it'll use the next `fd` instead of closing it

- When `dup()`, `dup2()`, or `fcntl()` are used to create a duplicate of a file descriptor, the close-on-exec (`O_CLOEXEC`) flag is always reset for the duplicate `fd`.

### `execve()` and file descriptors
- `execve()` is the only way Linux kernel can start new program
- Syscall executes a binary file if first argument is an `ELF` compiled file and has an executable bit set, or starts an interpreter with the content of the file if argument has a hashbang (e.g. `#!/usr/bin/python`) on first line of file and has an exec bit set
- After an `execve()` call a file offsets and flags are copied and shared if close-on-exec (`O_CLOEXEC`) flag is not set


- To protect system from leaking `fd` from parent to child, one can use `close_range()` syscall to close open file descriptors
- `O_CLOEXEC` should still be used because it's hard to track opened file from main program (for e.g. libraries)
- Another case is when `exec()` fails and user still requires some previously opened files. Reopening files after an error is hard

Stopped reading [here](https://biriukov.dev/docs/fd-pipe-session-terminal/1-file-descriptor-and-open-file-description/#:~:text=Check%20if%202%20file%20descriptors%20share%20the%20same%20open%20file%20description%20with%20kcmp())