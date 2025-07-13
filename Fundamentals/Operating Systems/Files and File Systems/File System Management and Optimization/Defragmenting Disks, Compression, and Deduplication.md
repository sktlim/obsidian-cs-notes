## Defragmenting Disks
When OS is initially installed, programs and files are installed consecutively starting at the beginning of disk. However, disks become fragmented over time as files are created and removed. Blocks used for a file may be all over disk, resulting in degraded performance.

Performance can be restored by **defragmenting** the disk
- Files are moved around to make them contiguous and put all or most of free space into a large contiguous region on disk
- Windows uses `defrag` for this and users should use this frequently, unless they're using SSDs

Defragmentation works better on file systems that have a lot of free space in a contiguous region at end of partition
- Space allows defragmentation program to select fragmented files near start of partition and copy all their block to the free space, which frees up contiguous block of space near start of partition which other files can be placed contiguously
- Some system files cannot be moved
	- This includes paging file, hibernation file, journaling log
	- Lack of mobility is a problem when file happens to be near end of partition and user wants to reduce partition size
	- Solution: Remove them, resize partition, recreate them

## Compression
- Compression Algorithms looks for repeating sequences of data that can be encoded efficiently
	- See [[Lempel-Ziv]]
## Deduplication
- Several popular file systems remove redundancy across files through deduplication
- File system deduplication possible at the granularity of files, portions of files, or individual disk blocks
- When deduplication procedure detects 2 files contain chunks that are exactly the same, it will keep only a single physical copy that's shared by both files. This is done inline or post-process
	- **Inline**: 
		- File system calculates hash for every chunk that it's about to write and compares it to hashes of existing chunks. 
		- If chunk is already present, it will refrain from actually writing data and reference existing data chunk instead
		- Additional calculations take time and slows down write
	- **Post-process**:
		- Writes out data and performs hashing and comparisons in the background, avoiding slowing down file operations of process
	- There is slight chances of hash collision, but this is glossed over in the book.

