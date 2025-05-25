>To implement the main function of mapping path names to information needed to locate the file

- Some systems directly store attributes (file's owner, creation time) in directory entry itself
- For systems that use inodes, directory entry can just be the file name and inode number (as attributes can be stored in the inodes)

## Implementing Variable-length File Names
Implicit Assumption: Filenames are small and of fixed length, but this is not true in modern OSes
- How are long and variable-length path names supported?
![[Handling variable-length filenames.png|500]]
### Idea (a): 
- Don't treat all directory entries as the same size
	- Directory entry can have a fixed-length header, followed by the file name (however long) in big-endian format
	- Each file name terminated by a special character, and filenames need to be filled out to end at word boundary (shaded boxes)
#### Cons
- When file is removed, variable-sized gap introduced into directory in which the next file may not fit (same issue as [[Disk Block Allocation#Cons]], except now its solvable because directory is in memory)

### Idea (b): 
- Make directory entries fixed-length and keep all variable-length filenames in a heap at the end

#### Pros
- When entry removed, next entry always fit
- No need for filenames to begin at word boundary
#### Cons
- Heap must be managed and page faults might still occur


All designs so far use linear searching, which might be slow for big directories
- A hash table (in each directory) can be used to speed up
	- Filename hashing with closed-addressing can be used
	- More complex administration required and only a serious candidate in directories containing hundreds to thousands of files
- Caching directory search results can also be used to speed up searching