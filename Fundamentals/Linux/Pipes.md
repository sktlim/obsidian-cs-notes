- Allows user to build one-directional communication channels between related processes
- Can think of pipe as memory buffer with a **byte stream** API. 
	- By default, there are no message or strict boundaries (but this has changed since kernel 3.4 where `O_DIRECT` flag and packet mode were introduced)
- Another important feature is max size of an atomic write. `PIPE_BUF` constant determines it and sets it to 4096 bytes
- Reader and writer can use completely different user-space buffer sizes if they want. All written bytes are read sequentially, so making `lseek()` syscall for pipe is impossible

- Write calls to a pipe block if internal kernel buffer is full; Writer will block or return `EAGAIN` (if in non-blocking mode) until sufficient data has been read from pipe to allow writer to complete
- On the other hand, if all pipe readers close their file descriptors, writer will get `SIGPIPE` signal from kernel, and all subsequent `write()` calls will return `EPIPE` error

- From reader's perspective, pipe can return zero size read(`EOF`) if all writers close all their write pipe file descriptors
- Reader blocks if there is nothing to read until data is available

### Properties
- Internally, pipe buffer is a **ringer buffer** with slots
- Each slot has a size of a `PIPE_BUF` constant
- Number of 