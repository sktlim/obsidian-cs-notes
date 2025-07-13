> **Resources:** Objects to which some process has been granted exclusive access, e.g. devices, data records, files. Anything that must be acquired, used, and released over the course of time

## Preemptable and Non-preemptable resources
**Preemptable Resources:** resources than can be taken away from the process owning it with no ill effects. Example is memory, because pages can always be swapped out to SSD/disk to recover it.
**Non-preemptable Resources:** Cannot be taken away from current owner without potentially causing failure. 

Deadlocks involve **non-preemptable** resources.

## Dining Philosophers Problem
![[Dining Philosopher.png|300]]
### Problem Context
5 philosophers sit around a circular dining table. Each philosopher has two activities:
- **Thinking**
- **Eating**
Each philosopher has a plate of spaghetti in front of them and **one fork between each pair of adjacent philosophers**. Thus, with five philosophers, there are five forks. To eat spaghetti, a philosopher needs **two forks**: the one to their **left** and the one to their **right**.
### Rules
- A philosopher can **only eat** when they hold **both forks**.
- Philosophers **cannot communicate** with each other directly.
- Philosophers alternate between **thinking** and **eating**.
- They must **pick up one fork at a time**.
### Problem
#### Deadlock
- Occurs if each philosopher picks up their **left fork simultaneously** and waits indefinitely for their right fork. All philosophers are waiting and no progress can be made.
- Example:
	1. All philosophers simultaneously pick up their left fork.
    2. Each philosopher waits indefinitely for their right fork (already picked up by their neighbor).
    3. No philosopher can eat, causing **deadlock**.
#### Starvation 
Occurs if at least one philosopher is **unable to eat indefinitely**, while others continue to eat, leading to unfair resource distribution.
- Example:
	- **P1** and **P3** quickly finish thinking and pick up both forks to start eating.
	- Once they finish eating, **P1** and **P3** put down the forks, and immediately afterward, **P2** and **P4** pick up forks to eat.
	- This alternating pattern continues. When **P2** and **P4** finish, **P1** and **P3** eat again, followed again by **P2** and **P4**, and so forth.
	- Throughout this process, **P5** continuously attempts to pick up forks but never successfully acquires both at the same time because the forks on either side are always occupied when **P5** tries to eat.
### Solution
