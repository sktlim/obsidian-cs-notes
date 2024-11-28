## Description
- **Condition variables** allow threads to wait for a condition/ state change
- Typically used with mutexes

## Examples
- [[Producer-Consumer Problem]]

## For Linux

| Thread Call              | Description                                  |
| ------------------------ | -------------------------------------------- |
| `pthread_cond_init`      | Create a condition variable                  |
| `pthread_cond_destroy`   | Destroy a condition variable                 |
| `pthread_cond_wait`      | Block waiting for a signal                   |
| `pthread_cond_signal`    | Signal another thread and wake it up         |
| `pthread_cond_broadcast` | Signal multiple threads and wake all of them |