> Shared variable containing two states: locked or unlocked

- Mutexes are commonly used together with condition variables
	- Pattern is for a thread to lock mutex, then wait on a condition variable when it cannot get what it needs. Eventually another thread signals it and it continues.

- Can be easily implemented in user space provided that TSL or XCHG instructions are available

```asm
mutex_lock:
	TSL REGISTER,MUTEX             | copy mutex to register and set mutex to 1
	CMP REGISTER,#0                | was mutex zero?
	JZE ok                         | if it was zero, mutex was unlocked, so return
	CALL thread yield              | mutex is busy; schedule another thread
	JMP mutex lock                 | try again
	ok: RET                        | return to caller; critical region entered

mutex_unlock:
	MOVE MUTEX,#0                  | store a 0 in mutex
	RET                            | return to caller
```

- For user threads, there is a difference between `enter_region` in [[Race conditions and Critical Region Solutions]] and mutexes. The former is controlled by the scheduler and is a form of **spinlock** while the latter typically calls `thread_yield` to give up CPU when it fails to acquire a lock
- `mutex_lock` and `mutex_unlock` do not require kernel calls and are therefore fast
