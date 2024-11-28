> High-priority task is blocked because a lower-priority task holds a resource it needs, causing the high-priority task to wait indefinitely

## How Priority Inversion Occurs
1. **Scenario Setup**:
    - Three tasks (or threads) with different priorities: **High**, **Medium**, and **Low**.
    - The **Low-priority** task acquires a resource (e.g., a lock) that both **High** and **Medium** tasks need.
2. **Priority Inversion Sequence**:
    - The **High-priority** task becomes ready to run and attempts to acquire the resource held by the **Low-priority** task.
    - Because the **Low-priority** task holds the lock, the **High-priority** task must wait until the **Low-priority** task releases it.
    - A **Medium-priority** task becomes ready to run and, due to its higher priority over the **Low-priority** task, preempts it.
    - Now, the **High-priority** task is effectively blocked by the **Medium-priority** task, which should not have affected it in a well-prioritized system.
3. **Result**:
    - The **High-priority** task is indirectly delayed by the **Medium-priority** task because the **Low-priority** task is preempted and unable to release the needed resource.
    - This is a priority inversion, as a lower-priority task is blocking a higher-priority one, reversing the intended order of execution.
## Solutions
- Disable all interrupts in critical region
	- Lower priority task cannot hold resource indefinitely and will release
	- Not ideal as mentioned in [[Race conditions and Critical Region Solutions]]
- **Priority Ceiling**
	- Associate priority to mutex and assign that priority to process holding it
	- As long as no process wanting to grab mutex has higher priority than the ceiling, inversion is not possible
	- E.g. Assign mutex high priority. If low-priority process holds onto it, low-priority is assigned high priority. Medium priority can no longer grab it
- **Priority Inheritance**
	- Low priority task holding mutex will temporarily inherit priority of high priority task trying to obtain it
	- No medium priority task will be able to preempt task holding mutex
- **Random boosting**
	- Rolling dice and giving random mutex-holding threads high priority until they exit critical region
	- Used by OS like Microsoft Windows