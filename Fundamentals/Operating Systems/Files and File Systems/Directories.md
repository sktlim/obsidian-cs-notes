>**Directories:** Special files in a file system that act as containers to organize and store references (pointers) to other files and directories, forming a hierarchical structure

## Types
- **Single-level directory systems**
	- Still in use in embedded systems
	- Single root directory
- **Hierarchical Directory systems**

## Path Names
- **Absolute path names:** Path from root directory to file
	- UNIX: separated by `/`
	- Windows: separated by `\`
- **Relative path name:** Path name from **current working directory**

- Each process has its own working directory 
- Most OS have `.` (refers to current directory) and `..` (refers to parent)

## Directory Operations
- Create
	- `mkdir()`: makes empty directory except for `.` and `..`
- Delete
	- `rmdir()`: Deletes only an empty directory
- Open Directory
	- analogous to opening a file to read its contents
- Close Directory
	- After reading, directories should be closed to free up space
- Read Directory
	- `readdir()`: Returns next entry in open directory. `readdir()` only returns 1 entry in a fixed format
	- Used to be able to just use `read()`, but it required programmer to know internal directory structure
- Rename
- Link
	- `link(existing_file, path_name)`: Allows file to appear in more than one directory
	- Creates link from existing file to name specified by path
	- Increments counter in file's inode
	- Known as a **hard link**
- Unlink
	- Remove directory entry
		- If file is only present in one directory, it will just be removed from file system
		- If present in multiple directories, only path name specified will be removed

**Symbolic Link/ Shortcut/ Alias:** Name created pointing to tiny file naming another file