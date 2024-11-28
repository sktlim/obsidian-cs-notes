- While semaphores and monitors are useful, semaphores only work when a bunch of CPUs have access to shared memory, and monitors are only usable in a few programming languages
- None of the synchronization primitives allow for info exchange between machines

## Message Passing
- Message passing seeks to solve this issue of info exchange between machines
- Uses 2 primitives `send(destination, &message)` and `receive(source, &message)` which are system calls
	- If no message available, receiver can block until one arrives
	- Alternatively, can return with error code

### Design issues with Message Passing
- Messages can be lost on network
	- **Solution:** Use acknowledgement like in [[TCP]]
- Process naming so that process specified is unambiguous
- Authentication is also an issue
- Performance

