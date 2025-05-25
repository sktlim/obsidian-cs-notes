>Abstraction mechanism for dealing with data storage on computers
## File Extensions
- In UNIX, file extensions are just conventions and are not enforced by OS
- In Windows, OS is aware of file extensions and assigns meaning to them
	- Users can register extensions with OS and specify which program "owns" the extension

## File Structure
![[Types of File Structures.png|600]]
### Unstructured Sequence of Bytes
- OS only sees bytes; Used by both UNIX and Windows

### Sequence of fixed-length Records
- R/W operations only apply to 1 record at a time
- Common model on mainframe punched card computers 

### Tree 
- Tree of records, each containing a **key** field in a fixed position on the record
- Tree sorted on key field

## File Types
- **Regular Files**: Files containing user information
- **Directories:** System files for maintaining structure of file system
- **Character Special Files:** Files dealing with I/O and used to model serial I/O devices
- **Block Special Files:** Files used to model disks

See [[Special files]] for more info

### Regular Files
- Either ASCII or binary
- ASCII: Consists of lines of text
	- Each line terminated by either **carriage return character** `\r` or **line feed character** `\n`
	- Window uses both i.e. `\r\n`
- Binary: Just means not ASCII

**Executable binary file and archive file example**
![[Executable File Example.png|500]]
- **Magic Number:** Unique sequence of bytes embedded in a file to identify type and format. See [here](https://en.wikipedia.org/wiki/List_of_file_signatures)

- Archive file: Consists of a selection of library procedures compiled but not linked

## File Access
- **Sequential Access**: Process could read all bytes in order, but could not read out of order
	- Used when storage medium were magnetic tapes instead of disks
- **Random Access:** Process can read any byte or access records by key rather than position

## File Attributes/ Metadata
![[File Attributes.png|500]]
- **Temporary flag:** Allows file to be marked for automatic deletion when process terminates
- **Maximum size:** Mainly used on old mainframe systems
## File Operations (in Linux context)
- Create
	- `creat()` or `open()` with `O_CREATE` flag set
	- File created with no data. Initializes metadata attributes
- Delete
	- `delete()` or `unlink()`
	- Removes file entry from directory. Disk space only freed when no processes are using file
- Open
	- `open()`
	- Loads file attributes and disk location data into memory
- Close
	- `close(fd)`
	- Closes file descriptor `fd`, freeing kernel resources
	- Ensures any pending writes are flushed to disk
- Write
	- `write(fd, data, strlen(data))`
	- Writes data to file at current position
	- Overwrites existing data at position
- Append
	- `open()` with `O_APPEND` flag set
	- Append data to end of file
- Seeking
	- `lseek(fd, 0, SEEK_SET) // move file pointer to start of file`
	- Repositions file pointer 
- Get attributes
	- `stat()`, `fstat()`
	- Retrieves file attributes
- Set attributes
	- `chmod()`, `chown()`, `utime()`
- Rename
	- `rename()`
	- Rename file without copying its data