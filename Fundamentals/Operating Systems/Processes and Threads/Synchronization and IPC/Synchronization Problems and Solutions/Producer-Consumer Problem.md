- Also known as **bounded-buffer problem**

## Problem
- 2 processes (producer _P_ and consumer _C_) share a common, fixed size buffer
- _P_ puts info into the buffer and _C_ takes info out
- **Problems:**
	- Buffer is full and producer wants to put items in, OR
	- Buffer is empty and consumer wants to take items out

- **Preliminary Intuition:** _P_/ _C_ goes to sleep until _C_/_P_ takes/ produces something into the buffer
```c
#define N 100          /* number of slots in the buffer */
int count = 0;         /* number of items in the buffer */
void producer(void)
{
	int item;
	while (TRUE) {                        /* repeat forever */
		item = produce item( );           /* generate next item */
		if (count == N) 
			sleep( );                     /* if buffer is full, go to sleep */
		insert item(item);                /* put item in buffer */
		count = count + 1;                /* increment count of items in buffer */
		if (count == 1) 
			wakeup(consumer);             /* was buffer empty? */
	}
}

void consumer(void)
{
	int item;
	while (TRUE) {                        /* repeat forever */
		if (count == 0) 
			sleep( );                     /* if buffer is empty, got to sleep */
		item = remove item( );            /* take item out of buffer */
		count = count 􀀼 1;                /* decrement count of items in buffer */
		if (count == N 􀀼 1) 
			wakeup(producer);             /* was buffer full? */
		consume item(item);               /* print item */
	}
}
```
- **Problem:** `count` is unconstrained and might cause a race condition

- **Scenario**: 
	- Empty buffer, consumer checks count if 0
	- Context switch to producer before `sleep()`
	- Producer produces item and places in buffer (`count=1`) and calls `wakeup(consumer)`
	- `wakeup` lost as consumer is awake
	- Context switch to consumer
	- Consumer continues and goes to sleep
	- Nothing is consumed from the buffer, and eventually producer goes to sleep
	- Both sleep forever

- **Quick fix:** Add **wakeup waiting bit** 
	- When `wakeup()` is sent to awake process, `wakeup_waiting` is set to 1
	- When process tries to go to sleep, if `wakeup_waiting` is 1, it will be set to 0 and process remains awake
	- Basically a store for wakeup signals
	- However, with 3 or more processes this quick fix becomes insufficient

## PCP with Synchronization Primitives
### Solution using Semaphores
```c
#define N 100 /* number of slots in the buffer */
typedef int semaphore; /* semaphores are a special kind of int */
semaphore mutex = 1; /* controls access to critical region */
semaphore empty = N; /* counts empty buffer slots */
semaphore full = 0; /* counts full buffer slots */

void producer(void)
{
	int item;
	while (TRUE) {                       /* TRUE is the constant 1 */
		item = produce item();          /* generate something to put in buffer */
		down(&empty);                    /* decrement empty count */
		down(&mutex);                    /* enter critical region */
		insert item(item);              /* put new item in buffer */
		up(&mutex);                      /* leave critical region */
		up(&full);                       /* increment count of full slots */
	}
}

void consumer(void)
{
	int item;
	while (TRUE) {                        /* infinite loop */
		down(&full);                      /* decrement full count */
		down(&mutex);                     /* enter critical region */
		item = remove item();            /* take item from buffer */
		up(&mutex);                       /* leave critical region */
		up(&empty);                       /* increment count of empty slots */
		consume item(item);               /* do something with the item */
	}
}
```

- Solution uses **binary semaphores** mutex and 2 **counting semaphores** empty and full
- If each process does a down just before entering its critical region and an up just after leaving it, mutual exclusion is guaranteed.

### Solution using mutexes and condition variables
```c
#include <stdio.h>
#include <pthread.h>
#define MAX 1000000000 /* how many numbers to produce */

pthread mutex_t mutex;
pthread cond_t condc, condp; /* used for signaling */
int buffer = 0; /* buffer used between producer and consumer */

void *producer(void *ptr) /* produce data */
{ 
	int i;
	for (i= 1; i <= MAX; i++) {
		pthread_mutex_lock(&mutex);            /* get exclusive access to buffer */
		while (buffer != 0) 
			pthread_cond_wait(&condp, &mutex);
		buffer = i;                            /* put item in buffer */
		pthread_cond_signal(&condc);           /* wake up consumer */
		pthread_mutex_unlock(&mutex);          /* release access to buffer */
	}
	
	pthread_exit(0);
}
void *consumer(void *ptr) /* consume data */
{ 
	int i;
	for (i = 1; i <= MAX; i++) {
		pthread_mutex_lock(&mutex); /* get exclusive access to buffer */
		while (buffer ==0 ) 
			pthread_cond_wait(&condc, &mutex);
		buffer = 0; /* take item out of buffer (not shown) and reinitialize */
		pthread_cond_signal(&condp); /* wake up producer */
		pthread_mutex_unlock(&the mutex); /* release access to buffer */
	}
	pthread_exit(0);
}
int main(int argc, char **argv)
{
	pthread_t pro, con;
	pthread_mutex_init(&mutex, 0);
	pthread_cond_init(&condc, 0);
	pthread_cond_init(&condp, 0);
	pthread_create(&con, 0, consumer, 0);
	pthread_create(&pro, 0, producer, 0);
	pthread_join(pro, 0);
	pthread_join(con, 0);
	pthread_cond_destroy(&condc);
	pthread_cond_destroy(&condp);
	pthread_mutex_destroy(&mutex);
}
```
- Note that this solution uses a **single-item buffer** (only one item can be held at one time)
	- Producer must wait until item consumed before producing new item
	- **Strict alternation** is forced


#### Trade-offs Between Semaphores and Mutexes with Condition Variables
Assuming both are bounded buffers and only difference in implementation is whether they are using semaphores or mutexes

|Aspect|Semaphores|Mutexes and Condition Variables|
|---|---|---|
|**Counting and Buffer Management**|Naturally suited for counting empty/full slots|Requires manual counting and condition checks|
|**Blocking and Waking**|Implicitly handles waiting threads|Explicit `while` loop checks ensure condition|
|**Ease of Use**|More compact and concise for simple cases|More readable and explicit logic, easier to debug|
|**Control Over Waking Threads**|Less fine-grained control|Fine-grained control, better for custom logic|
|**Spurious Wakeups**|Needs careful handling if they occur|Handled within the `while` loop rechecks|
|**Performance in High Contention**|Can be more efficient in straightforward cases|Potentially more efficient in complex cases|

### Producer-Consumer Problem using Monitors
```java
public class ProducerConsumer {
    static final int N = 100; // constant giving the buffer size
    static Producer p = new Producer(); // instantiate a new producer thread
    static Consumer c = new Consumer(); // instantiate a new consumer thread
    static OurMonitor mon = new OurMonitor(); // instantiate a new monitor

    public static void main(String args[]) {
        p.start(); // start the producer thread
        c.start(); // start the consumer thread
    }

    static class Producer extends Thread {
        public void run() { // run method contains the thread code
            int item;
            while (true) { // producer loop
                item = produceItem();
                mon.insert(item);
            }
        }

        private int produceItem() { 
            // Actual item production logic here
            return (int) (Math.random() * 100); // example random item
        }
    }

    static class Consumer extends Thread {
        public void run() { // run method contains the thread code
            int item;
            while (true) { // consumer loop
                item = mon.remove();
                consumeItem(item);
            }
        }

        private void consumeItem(int item) { 
            // Actual item consumption logic here
            System.out.println("Consumed item: " + item);
        }
    }

    static class OurMonitor { // this is a monitor
        private int[] buffer = new int[N];
        private int count = 0, lo = 0, hi = 0; // counters and indices

        public synchronized void insert(int val) {
            while (count == N) goToSleep(); // if the buffer is full, go to sleep
            buffer[hi] = val; // insert an item into the buffer
            hi = (hi + 1) % N; // slot to place next item in
            count = count + 1; // one more item in the buffer now
            if (count == 1) notify(); // if consumer was sleeping, wake it up
        }

        public synchronized int remove() {
            int val;
            while (count == 0) goToSleep(); // if the buffer is empty, go to sleep
            val = buffer[lo]; // fetch an item from the buffer
            lo = (lo + 1) % N; // slot to fetch next item from
            count = count - 1; // one fewer item in the buffer
            if (count == N - 1) notify(); // if producer was sleeping, wake it up
            return val;
        }

        private void goToSleep() { 
            try { 
                wait(); 
            } catch (InterruptedException exc) {
                Thread.currentThread().interrupt(); // handle interruption
            }
        }
    }
}
```
- Advantage of this solution is because of `OurMonitor`, we can assume that insert and remove are mutually exclusive 

## PCP with Message Passing
```c
#define N 100 /* number of slots in the buffer */
void producer(void)
{
	int item;
	message m; /* message buffer */
	while (TRUE) {
		item = produce_item();      /* generate something to put in buffer */
		receive(consumer, &m);      /* wait for an empty to arrive */
		build_message(&m, item);    /* construct a message to send */
		send(consumer, &m);         /* send item to consumer */
	}
}

void consumer(void)
{
	int item, i;
	message m;
	for (i = 0; i < N; i++) 
		send(producer, &m);          /* send N empties */
	while (TRUE) {
		receive(producer, &m);       /* get message containing item */
		item = extract_item(&m);     /* extract item from message */
		send(producer, &m);          /* send back empty reply */
		consume_item(item);          /* do something with the item */
	}
}
```
- Variants are possible with message passing
	- **Mailbox:** Data structure to buffer a certain number of messages
		- When mailboxes used, address params in send and receive are the mailboxes, not the processes themselves
	- **Rendezvous (Eliminate all buffering):** if `send` happens before `receive`, sending process blocked until `receive` can happen, at which time messages copies directly from sender to recipient
	- Well-known message passing system is the **Message-Passing Interface (MPI)**
## General Case Producer-Consumer Problem Solution
```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>
#include <chrono>

std::mutex mtx;
std::condition_variable cv;
std::queue<int> buffer;
const unsigned int BUFFER_SIZE = 10;

// Producer function
void producer(int id, int num_items) {
    for (int i = 0; i < num_items; ++i) {
        std::unique_lock<std::mutex> lock(mtx);
        
        // Wait until there is space in the buffer
        cv.wait(lock, [] { return buffer.size() < BUFFER_SIZE; });
        
        // Produce an item and add it to the buffer
        int item = i + 1;
        buffer.push(item);
        std::cout << "Producer " << id << " produced item " << item << std::endl;
        
        // Notify the consumer that there is a new item
        cv.notify_all();
    }
}

// Consumer function
void consumer(int id, int num_items) {
    for (int i = 0; i < num_items; ++i) {
        std::unique_lock<std::mutex> lock(mtx);
        
        // Wait until there is an item in the buffer
        cv.wait(lock, [] { return !buffer.empty(); });
        
        // Consume an item from the buffer
        int item = buffer.front();
        buffer.pop();
        std::cout << "Consumer " << id << " consumed item " << item << std::endl;
        
        // Notify the producer that there is space in the buffer
        cv.notify_all();
    }
}

int main() {
    int num_items = 20; // Total items each producer and consumer should produce/consume
    std::thread p1(producer, 1, num_items);
    std::thread p2(producer, 2, num_items);
    std::thread c1(consumer, 1, num_items);
    std::thread c2(consumer, 2, num_items);
    
    // Wait for all threads to finish
    p1.join();
    p2.join();
    c1.join();
    c2.join();

    return 0;
}

```

## In Summary

If the goal is to implement a simple, **efficient bounded buffer** with minimal code, semaphores are often preferred, as they naturally handle counting for buffer slots. They reduce complexity by implicitly blocking and unblocking threads based on slot availability. However, if the application requires **fine-grained control** or has complex conditions on when to wake up specific threads, then using **mutexes and condition variables** offers better control, readability, and is often easier to maintain in complex systems.