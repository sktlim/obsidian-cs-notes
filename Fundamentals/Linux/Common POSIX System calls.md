See `man` pages for more in-depth details, what's here is only meant as a rough refresher

## Process management
### `fork()` -> `pid`
- **only way to create new process in POSIX**
- Creates exact duplicate, including all file descriptors, registers
- Memory of child may be shared **copy-on-write** with parent, meaning that parent and child share single physical copy of memory until one of them modifies a value at a location, in which case OS makes a copy of the memory containing that location
	- Minimizes the memory that needs to be copied
- RETURN VALUE
       On  success, the PID of the child process is returned in the parent, and 0 is returned in the child.  On failure, -1 is returned in the parent, no child process is created, and errno is set appropriately.

### `waitpid(pid, &statloc, options)` -> `pid`
- **waits for child to terminate**
- can wait for specific child, or for any old child by setting `pid = -1` 
- When child finishes, address pointed to by `statloc` will be set to child process' exit status (normal/ abnormal termination/ exit value)
- Options provided include return immediately if no child has already exited

### `execve(name, argv, environp)` -> void (success); -1 (fail)
- **Replace a process' core image**
- `name`: name of file to be executed
- `argv`: pointer to argument array
- `environp`: pointer to environment array
- Example usage: When a shell forks a process to execute user command, process calls `execve` to replace its core image with the file specified in `name` and subsequently execute it


## File Management
### `open(file, how,...)` -> `fd`
- Opens a file for reading/writing/both and returns the file descriptor `fd` for usage

### `close(fd)` -> 0 (success) / -1 (error)
- Closes open file `fd`

### `read(fd, buffer, nbytes)` -> bytes read / -1 (error)
- read `nbytes` from `fd` into `buffer` 
- RETURN VALUE
       On  success,  the  number of bytes read is returned (zero indicates end of file), and the file position is advanced by this number.

### `lseek(fd, offset, whence)` -> absolute position in file
- `fd`: file descriptor
- `offset`: file position
- `whence`: Whether file position is relative to beginning of file, current position, or EOF


### `stat(name, &buf)` 
- Gets file `name`'s information e.g. regular/special file, permissions, last modified, directory
- `fstat` does the same for open file

## Directory management
### `mkdir(name, mode)`, `rmdir(name)`
- Create and remove empty directories
- `mode` is file mode aka file perms

### `link(name1, name2)`
- Create new entry `name2` pointing to `name1`
- Allow same file to appear under 2 or more names, often in different directories
- Does this by basically creating new directory entry with [[inode]] number of existing file
- Usage: Allow members of same team to access same file in their own directories (and maybe under different names)
### `unlink(name)`
- Remove directory entry 

### `mount(special, name, flag)`, `unmount(special)`
- Mount/ unmount file system
- `special`: name of a block special file for removable media (for e.g.)
- `name`: place in tree to be mounted
- `flag`: mounted read-only or write-only
- Merges filesystems into one (e.g. if I mount USB `/dev/sdb1/` to `/media/usb` on my fs, I just need to do `/media/usb/readme.md` instead of addressing  `/dev/sdb1/`)
- If multiple devices mounted into same directory, each new mount will "hide"/ "override" old one as each mount point can only represent one device at a time

## Miscellaneous system calls
### `chdir(dirname)`
- change working directory

### `chdmod(name, mode)`
- Change file's mode (protection bits). Prefixed with 0 to signify octal notation
- When applying permissions to files or directories, each of the three numbers represents a different level of access:
	- **First digit**: Owner permissions
	- **Second digit**: Group permissions
	- **Third digit**: Others’ permissions

|Numeric|Symbolic|Description|Permissions|
|---|---|---|---|
|0|`---`|No permissions|No read, write, or execute|
|1|`--x`|Execute only|Execute only|
|2|`-w-`|Write only|Write only|
|3|`-wx`|Write and execute|Write and execute|
|4|`r--`|Read only|Read only|
|5|`r-x`|Read and execute|Read and execute|
|6|`rw-`|Read and write|Read and write|
|7|`rwx`|Read, write, and execute|Full permissions|

- **Special Permissions**: Sometimes, permissions include special bits beyond the basic read, write, and execute:
	- **Setuid (4)**: When executed, the program runs with the **file owner's privileges**, not the user's. For files only.
	- **Setgid (2)**: Applies to files and directories
		- For files: Runs with the **group’s privileges**.
	    - For directories: New files inherit the **directory’s group**, not the creator’s.
	- **Sticky bit (1)**: Applies to **directories**. Only the **file owner** (or root) can delete or rename files, even if others have write access to the directory.

When special permissions are set, a **fourth digit** is added at the beginning, with values ranging from 1 to 7:
- `1xxx`: Sticky bit
- `2xxx`: Setgid
- `4xxx`: Setuid


### `kill(pid, signal)`
- Sends signal to process

### `time(&seconds)`
- Get elapsed time since Jan 1, 1970
