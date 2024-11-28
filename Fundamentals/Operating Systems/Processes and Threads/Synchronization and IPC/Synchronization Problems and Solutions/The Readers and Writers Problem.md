- Models access to a database

## Problem
- Imagine a database
	- Acceptable to have multiple processes reading database at one time
	- If one process is writing, no one else has access to database

## Solution: Reader-priority

```c
typedef int semaphore; /* use your imagination */
semaphore mutex = 1; /* controls access to rc */
semaphore db = 1; /* controls access to the database */
int rc = 0; /* # of processes reading or wanting to */

void reader(void)
{
	while (TRUE) {                     /* repeat forever */
		down(&mutex);                  /* get exclusive access to rc */
		rc = rc + 1;                   /* one reader more now */
		if (rc == 1) 
			down(&db);                 /* if this is the first reader ... */
			
		up(&mutex);                    /* release exclusive access to rc */
		read_database();              /* access the data */
		down(&mutex);                  /* get exclusive access to rc */
		rc = rc - 1;                   /* one reader fewer now */
		if (rc == 0) 
			up(&db);                   /* if this is the last reader ... */
			
		up(&mutex);                    /* release exclusive access to rc */
		use_data_read();               /* noncritical region */
	}
}
void writer(void)
{
	while (TRUE) {                  /* repeat forever */
		think_up_data();            /* noncritical region */
		down(&db);                  /* get exclusive access */
		write_database();           /* update the data */
		up(&db);                    /* release exclusive access */
	}
}
```
- Solution allows additional readers to enter if there is one reader, but writer might have starvation
- To avoid this, program could have queue where incoming readers wait behind writer if there is already a writer in queue. 
	- Writer would then have to wait for existing readers to finish but not for subsequent readers
	- This alternative solution however leads to writer priority and therefore less concurrency and less performance

## Solution: Fair Reader-Writer Priority
```cpp
semaphore mutex = 1;        // Protects access to readCount
semaphore writeLock = 1;    // Controls access for writers
semaphore queue = 1;        // Fair turnstile to manage request order
int readCount = 0;          // Number of active readers

// Reader Process
void reader() {
    down(queue);            // Wait in line for the fair turnstile
    down(mutex);            // Lock mutex to safely update readCount
    readCount++;
    if (readCount == 1) {   // First reader locks writeLock
        down(writeLock);
    }
    up(mutex);              // Unlock mutex after updating readCount
    up(queue);              // Release queue for the next process in line

    // Critical Section for Readers
    readResource();

    down(mutex);            // Lock mutex to safely update readCount
    readCount--;
    if (readCount == 0) {   // Last reader unlocks writeLock
        up(writeLock);
    }
    up(mutex);              // Unlock mutex
}

// Writer Process
void writer() {
    down(queue);            // Wait in line for the fair turnstile
    down(writeLock);        // Acquire exclusive access for writing
    up(queue);              // Release queue for the next process in line

    // Critical Section for Writer
    writeResource();

    up(writeLock);          // Release writeLock after writing
}

```