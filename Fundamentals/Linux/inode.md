> **Inode** (index node) is a data structure used by the filesystem to store info about files and directories, excluding actual file name/ file content

> Inodes themselves are stored on special area in disk called **inode table**. Table is typically allocated when filesystem is created 

![[Inode Diagram.png|500]]

- Each file or directory has a unique inode containing essential metadata
- Inodes can be viewed using the `stat` command
- Metadata includes
	- File metadata
		- File type: regular file, directory, symbolic link etc
		- Permissions
		- Owner and group IDs
		- Timestamps
			- `ctime`: Change time, indicating when metadata last changed 
			- `mtime`: Last Modified time of file content 
			- `atime`: Last Accessed time of file 
	- Size of file 
	- Data block pointers
		- References to actual blocks on disk where file data is stored 
	- Number of hard links
		- Count of how many directory entries reference inode. When count drops to 0, filesystem deletes file

- Every direct block pointer directly points to a data block. The singly indirect block pointer points to a block of pointers, each of which points to a data block. The doubly indirect block pointer contains another level of indirection, and the triply indirect block pointer contains yet another level of indirection.
- Each inode typically has:
	- 12 direct block pointers, 
	- 1 singly indirect block pointer, 
	- 1 doubly indirect block pointer, and 
	- 1 triply indirect block pointer. 
- 1 block is typically **4KB**, meaning that an inode can 
	- directly address **48kB** of data, 
	- **4MB** via singly indirect block, 
	- **4GB** via doubly indirect block,
	- **4TB** via triply indirect block

- Limit on number of inodes in a filesystem depends on filesystem type and configuration
	- In ext4, default config is **1 inode per 16kB** of disk space
	- Monitoring inode limit is thus important for filesystem with many small files and limited space