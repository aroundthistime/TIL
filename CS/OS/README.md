# OS (Operating System)

### 1. OS
- Program which is loaded by boot program, and manages all other application programs in a computer.
- `OS` will be in the middle of communication between hardware and the application program.


### 2. Kernel
- `Kernel` is the core part of `OS`, which will be on the memory of computer by boot program (because `OS` also is a heavy program, it cannot go on the memory entirely. Only the `Kernel` is brought, and the others will be called if necessary)
- `Kernel` will be an interface between `hardware` and the `software`:
    - **`Resource Management`**: Manage **system resources (eg. `CPU`, `Memory`, `Disk space`)** - includes **(de)allocation of resources while setting priorities between processes**.
    - **`Process Management`**: Manage **creation, termination, suspension, scheduling, and context switching of processes** and also communication between them.
    - **`System Call Management`**: Handles system calls to allow applications access to hardware resources (so basically an **interface between the system and the hardware**)
        - **`System Call` is an interface for users or programs to use `Kernel` provided features (OS)**. Programs will create `System Calls` and `Kernel` will execute them.
        - `System Call` limits the possible ways of using `Kernel` provided services **to protect computer resources**.
    - **`System Security`**: Manages **access control and permissions** to ensure the security of the system.


### 3. Process vs Thread
- **`Process`**
    - An **instance of program executed by one or more threads**.
    - Gets **allocated a separate resource by `Operating System`** so that processes are less likely to interfere with each other.
    - `Process` has **at least one thread (`main thread`)**.
    - In some cases (eg. Program which runs by `multi-process`), **multiple `processes` need to interact with each other**s. This is done by `IPC (Inter-process Communication)`. There could be 2 major ways of enabling communication between processes:
        - **`Message Passing`**: Pass messages between processes via `Kernel` (has overhead)
        - **`Shared Memory`**: Sharing memory between processes (no need to call system call but has several issues - `Producer-Consumer Issue`, `Critical Section`, ..)
    - The execution context of `process` will be stored inside data structure called `PCB (Process Control Block)`.

- **`Thread`**
    - Smallest sequence of programmed instructions that can be managed independently by a `OS scheduler`.
    - **Each `Thread` has its own `Stack`, but shares the rest (eg. `code`, `data`, `heap`) with other threads in the same process**.
        - `Threads` do not share `Stacks` because the structure of `Stack` which contains function call information would get too complex.
        - Because `threads` share resources except for `stack`, they can know the execution result of other `threads` immediately, but requires resource controls.
    - **Information about a certain thread will be stored inside `Kernel` in a `data structure` called `TCB (Thread Control Block)`**
        - contains: thread identifier, stack pointer, program counter, ..
        - **After `Context Switching`, the values could be used to resume the `thread task`**.

- **`Multi-process` vs `Multi-Thread`**
- `Multi-process` and `Multi-thread` are not mutually exclusive concepts, a program can be `multi-process` and `multi-thread` at the same time.
- Each tab of `Chrome` browser is a `process` (`Multi-process`) since errors on each tabs do not affect each other.<br>

<table>
    <thead>
        <tr>
            <th> </th>
            <th>Multi-Process</th>
            <th>Multi-Thread</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>Definition</th>
            <tr>
                Program consisting of multiple processes
            </tr>
            <tr>
                Program, the process of which consists of multiple threads
            </tr>
        </tr>
        <tr>
            <th>Advantages</th>
            <tr>
                - Error on certain process does not affect the other.
            </tr>
            <tr>
                - Less memory consume (less <strong>System Call</strong> for memory allocation, <strong>less context switching overhead</strong>)
            </tr>
        </tr>
        <tr>
            <th>Disadvantages</th>
            <tr>
                - Processes cannot interact with each other directly because they do not share the memory (must use IPC)<br><br>
                - Context switching overhead (interrupt, clearing cache, ..)
            </tr>
            <tr>
                - Error with one thread could affect others<br><br>
                - Synchronization approach is required with shared resources
            </tr>
        </tr>
    </tbody>
</table>


### 4. Multi-CPU, Multi-Core, Multi-Tasking
- Multi-CPU: Having multiple `CPUs` and have them all connected to the `memory`.
- Multi-Core: Implement multiple cores of `registers` and `cache` inside a `CPU`.
- Multi-Processing: Concurrent execution of multiple tasks. The tasks could interrupt each other and share resources (eg. `CPU`, `memory`)


### 5. Context Switching
- **`OS Kernel` changing the `process` or `thread` running on the `CPU core` into another**.

- Reasons of `Context Switching`:
    - **Pretend to execute multiple `processes` or `threads` simultaneously**.
    - **For fair CPU posession time** of each `process` or `thread`.
    - To **handle task with high priority first**.

- Triggers of `Context Switching`:
    - **Spent all `Time slice` (amount of time given to use `CPU`)**
    - Has **`I/O` data to process**. (Because `I/O` is slower than the speed of `CPU cycle`, do something else during `I/O` to prevent blocking and then process the result)
    - Waiting for a different resource or task.
    - **`Interrupt`**: Request to interrupt currently executing task (eg. to deal with events)

- Steps of `Context Switching`<br>
(1) **`Interrupt` occurs triggering `Context Switching`**. (could be hardware interrupt like `timer` or `I/O` / hardware interrupt like `system call` or `signal`)<br><br>
(2) **`CPU` suspends current task and gives control to the `Kernel`** (`OS`).<br><br>
(3) **`OS` saves context of the current state** (in **`TCB` or `PCB`** depending on whether it's a thread or process)<br><br>
(4) `OS` **updates scheduler to determine the next task** (`process` or `thread`).<br><br>
(5) `OS` **loads context of the next task from corresponding `TCB` or `PCB`**. The values are applied to `CPU register` (eg. `PC (Program Counter)`, `Stack Pointer`) - `Register` is a place to store information for processing tasks from the `CPU`<br><br>
(6) (Optional) If `Context Switch` involves updates in `memory address`, corresponding updates in `memory management structure` is required. (eg. when switching between `processes` with separate address spaces)<br><br>
(7) Resume execution<br><br>

- `Context Switching` between `processes` could require additional memory management and also **trigger `Cache Pollution` (`Cache memory` is a shared area between `processes`, but `cache data` from previous `process` is less likely to be used in the new `process`)**.


### 6. Concurrency (동시성) vs Parallelism (병렬성)
- **`Concurrency (동시성)`**
    - **Looks as if multiple programs are running at once, but is in fact handling multiple processes with `Context Switching`** (so two tasks cannot be processed at the same moment).
    - can be found at `Multi-Programming` with Single Core.

- **`Parallelism (병렬성)`**
    - Actually running multiple tasks at the same moment.
    - Usually requires CPU with multiple core.
    - `Parallelism` could be achieved even with single core `CPU` if the commands could run all at once (A core of CPU has multiple pipelines. If multiple tasks have no relevance with each other `Parallelism` could be achieved - eg. use different parts of CPU)


### 7. Critical Section (임계영역)
- **`Critical Section` is a protected section where concurrent access is denied** to prevent unexpected behaviors.
- **`Mutual Exclusion (상호 배제)`**: **Algorithm preventing simultanous access to shared resource**. Used by `Critical Section` to avoid issues caused by `Race Condition`
    - Key methods:
        - Critical Section **Enter**: Check if another `process` is accessing `Critical Section`, and wait if so.
        - Critical Section **Exit**: Postprocess for exiting `Critical Section` (eg. Mark flag that it exited `Critical Section`)
    
    - Requirements to fulfill when implementing `Mutual Exclusion` algorithm:
        - **`Processes` cannot enter `Critical Section` if in use**.
        - `Processes` **should be able to enter `Critical Section` if no `process` is using** it.
        - **`Processes` should wait finite time to enter `Critical Section`**

- **`Race Condition`**
    - `Race Condition` refers to a system where **multiple tasks would `race` on their own to access a shared resource**. If The **timing or the order of the access decides the system's behavior**, this could be an issue which leads to **unexpected or inconsistent result** of the program.

    -  How to prevent `Race Condition`:
        - **`Mutex (Mutual Exclusion Object)`**
            - `Mutex` is an `object` which ensures that **only one `thread` can access a resource at a time** so that data would remain consistent with integrity.
            - `Mutex` uses **`lock` and `unlock`** methods.
            - If a resource is locked, the thread has to wait till it is unlocked.
            - In most of the cases, `Mutex` is **used within a single process**.

        - **`Semaphore`**
            - **`Semaphore` is an `non-negative integer` which can express the number of `threads` that can access the resource** (will be initialized with maximum number of tasks that can access a certain resource simultaneously).
            - `Semphore` usually allows simultanoues access between `processes`.
            - **`wait`**: **If `Semaphore` is bigger than zero, `Semaphore` would be decreased and the operation will continue, otherwise will wait** till it becomes bigger than zero.
            - **`signal`**: **Increases `Semaphore` value**.


- **`DeadLock (교착 상태)`**
    - `DeadLock` refers to a situation that **tasks cannot proceed forever because they're waiting for another for itself**.
    - Conditions for triggering `DeadLock` (all conditions needs to be met. If at least one is not fulfilled, `DeadLock` will not happen):
        - **`Mutual Exclusion`**: Multiple Processes **cannot access a shared resource simultanouesly**.
        - **`Hold and Wait`**: `Process` is **currently holding at least one resource, and is requesting for another which is being held by another `process`**.
        - **`No pre-emption (비선점)`**: A `process` **cannot take away resource when another `process` is using it**.
        - **`Circulat Wait`**: `Processes` **have resources that another `process` requires in a circular way**.
    
    - How to handle `DeadLock`:
        - **`DeadLock Ignorance`**
            - `OS` **assuming that `Deadlock` will never happen**. (most widely used)
            - **The handling methods of `DeadLock` sacrifies performance while `DeadLock` does not occur so often**. Therefore `OS` like **`Windows` and `Linux` would just let the user restart the system when `DeadLock` occurs** and focus more on the performance.

        - **`DeadLock Prevention`**
            - **Prevent `DeadLock` from happening by excluding the condition that `DeadLock`** requires.
            - `Remove Mutual Exclusion`
                - Limitation
                    - While for several resources, it is possible to remove `Mutual Exclusion` (eg. `Spooling` for printers), it is not applicable to all types of resources.
                    - Removing `Mutual Exclusion` could trigger `Race Conditions`.

            - Remove `Hold and Wait` by assigning all the necessary resources at the beginning:
                - Limitation
                    -Practically impossible to define all the necessary resources initially.
                    - Inefficient cause the process would keep holding a resource that it doesn't actually need, but is required for others.

            - Allow `Preemption`
                - Limitation
                    - The result of the resource-stolen task will be inconsistent.

            - **Avoid `Circular Wait` by assigning priority to resources**
                - Method of **assigning priority number to each resource and a process cannot request resource with lower priority** → This will prevent `Circular Wait`
                - Unlike other `DeadLock Prevention` methods, this is considered to be a practical approach, but effort needs to be given on how to `prioritize` resources. (if resource prioritizing is done incorrectly, there could still be `DeadLocks`).

        - **`DeadLock Avoidance`**
            - Proactively **look for potential `DeadLocks` and avoid in advance** before they actually occur.
            - This **does not ensure that `DeadLock` will never happen, but avoid as much as possible**.
            - `Banker's Algorithm`
                - **Process should declare `maximum number of resources` it would request at the beginning**, and the **`system` would determine based on current resource allocation and other informations to see if granting the request would maintain the system in `safe state` (declined task will be postponed)**.
                - Has flaws that a task must declare all potential resource requests at the beginning, and because of that the allocation of the resource could be ineffective.

        - **`DeadLock Detection & Recovery`**
            - Let processes fall `DeadLock`, **periodically check if there's `DeadLock` and apply recovery** methods if one is detected. (eg. **`aborting` and `restarting` the process**)