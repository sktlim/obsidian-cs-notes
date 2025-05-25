>How can files be shared among different users? Answer: Linking

![[Shared File.png|350]]
When shared file needs to appear on multiple users' filesystems, the file system should become a Directed Acyclic Graph (DAG)

## Issue
- If directories contain disk addresses, copy of the disk address made when link is made. 
	- If subsequently a user appends to the file, new blocks will only be listed in the directory of the user doing the append and changes will not be visible to other user
## Solution
- Directories have pointer to inodes; Disk blocks listed in inode instead of directory (approach taken in UNIX)
	- Increases link count in inode
	- If owner C removes file, it will disappear in owner's directory but still exist as B is still pointing to it; Issue if system has disk quotas for each user, as file's owner is still C
	- Issue does not occur with symbolic links
- **Symbolic Linking**
	- System creates a `LINK` file in B's directory containing pathname to C's file
	- When B reads from `LINK` file, system recognizes it and reads C's file instead
	- When owner removes file, it is destroyed and subsequent attempts to read it will just fail to locate file
	- Symbolic links can be used to link files anywhere in the world if network address provided in front of the file's pathname
	
	- However, symbolic links require overhead (possibly more disk accesses) in reading pathnames from the `LINK` file to access the linked location
	- Extra inode and disk block (to store path) required for each symbolic link
- Even with symbolic links, there is the issue of possible duplication as well as files can have 2 or more paths. 
	- When copying the root directory, the program copying might make 2 copies of the same file due to linking
