> Collection of procedures, variables, and data structures grouped together in a kind of module or package. Processes can call procedures in monitor, but cannot directly access monitor's data structures

- Monitors are a language concept (C does not have them)
- Only 1 process from a monitor can be active at any instant
	- Calling process will check if any other process is active
- Condition variables should still be considered, as monitors only ensure mutual exclusion, and not guarantee that threads that cannot proceed will block

### Java support for monitors
- Java supports monitors by using the `synchronized` keyword on methods
	- Only 1 thread can execute a `synchronized` method of an object at any time
- Java lacks built-in condition variables; instead, it uses `wait` and `notify` for thread communication.
- These methods are only effective within synchronized methods, avoiding race conditions.