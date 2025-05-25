- Windows may have different file systems in use and handles this by assigning them a different driver letter (e.g. *C:, D:*)
- Linux tries to integrate and unify everything into 1 single structure through mounts (ext4 could be root filesystem while an ext3 partition could be mounted under */usr*)
- Main drive for VFS by Sun Microsystems was to support remote file systems using the **Network File System (NFS)** protocol

Most UNIX systems use concept of Virtual File System (VFS) to integrate multiple files into orderly structure

## How it works 
![[Virtual File System (VFS).png|500]]
**Key Idea:** Abstract out part of file system that is common to all file systems and put code in separate layer that calls underlying concrete file systems to actually manage data

- While most of the file systems under VFS represent partitions on a local disk, this is not always the case
- VFS design is such that as long as concrete file system supports the calls that VFS requires, it works
## Implementation 
- Several key object types usually supported
	- **Superblock:** Describes file system
	- **v-node**: Describes file
	- **Directory:** Describes directory
- Object types have associated operations they support, and VFS has some internal structures for its own use, including mount table and array of file descriptors 
## VFS Registration Process
- When system booted, root file system registered with VFS. Other file systems registered with VFS on mount 
- Registration basically means file system provides list of addresses to functions that are required by VFS, either as 1 call vector (table) or multiple of them, 1 per VFS object
- VFS then simply calls any function in the call vector to execute required operations

## VFS Usage
![[VFS Operation.png|500]]
- When process makes call to mounted filesystem, VFS first locates superblock of that filesystem
- VFS then creates v-node makes a call to that filesystem to return the info associated with file's inode 
- Info copied into v-node in RAM, along with call vector
- After copying, VFS makes an entry in the file descriptor table for calling process and sets it to point to the new v-node
- VFS return file descriptor to process so it can do its operations

- When process does an operation, VFS locates the v-node and follows function pointers, which it then calls to get the thing it needs