# OS (Operating System)

## 1. OS
- Program which is loaded by boot program, and manages all other application programs in a computer.
- `OS` will be in the middle of communication between hardware and the application program.


## 2. Kernel
- `Kernel` is the core part of `OS`, which will be on the memory of computer by boot program (because `OS` also is a heavy program, it cannot go on the memory entirely. Only the `Kernel` is brought, and the others will be called if necessary)
- `Kernel` will be an interface between `hardware` and the `software`:
    - **`Resource Management`**: Manage **system resources (eg. `CPU`, `Memory`, `Disk space`)** - includes **(de)allocation of resources while setting priorities between processes**.
    - **`Process Management`**: Manage **creation, termination, suspension, scheduling, and context switching of processes** and also communication between them.
    - **`System Call Management`**: Handles system calls to allow applications access to hardware resources (so basically an **interface between the system and the hardware**)
        - **`System Call` is an interface for users or programs to use `Kernel` provided features (OS)**. Programs will create `System Calls` and `Kernel` will execute them.
        - `System Call` limits the possible ways of using `Kernel` provided services **to protect computer resources**.
    - **`System Security`**: Manages **access control and permissions** to ensure the security of the system.


## 3. Process vs Thread
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
            <td>
                Program consisting of multiple processes
            </td>
            <td>
                Program, the process of which consists of multiple threads
            </td>
        </tr>
        <tr>
            <th>Advantages</th>
            <td>
                - Error on certain process does not affect the other.
            </td>
            <td>
                - Less memory consume (less <strong>System Call</strong> for memory allocation, <strong>less context switching overhead</strong>)
            </td>
        </tr>
        <tr>
            <th>Disadvantages</th>
            <td>
                - Processes cannot interact with each other directly because they do not share the memory (must use IPC)<br><br>
                - Context switching overhead (interrupt, clearing cache, ..)
            </td>
            <td>
                - Error with one thread could affect others<br><br>
                - Synchronization approach is required with shared resources
            </td>
        </tr>
    </tbody>
</table>


## 4. Multi-CPU, Multi-Core, Multi-Tasking
- Multi-CPU: Having multiple `CPUs` and have them all connected to the `memory`.
- Multi-Core: Implement multiple cores of `registers` and `cache` inside a `CPU`.
- Multi-Processing: Concurrent execution of multiple tasks. The tasks could interrupt each other and share resources (eg. `CPU`, `memory`)


## 5. Context Switching
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
(1) **`Interrupt` occurs triggering `Context Switching`**. (could be hardware interrupt like `timer` or `I/O` request / hardware interrupt like `system call` or `signal`)<br><br>
(2) **`CPU` suspends current task and gives control to the `Kernel`** (`OS`).<br><br>
(3) **`OS` saves context of the current state** (in **`TCB` or `PCB`** depending on whether it's a thread or process)<br><br>
(4) `OS` **updates scheduler to determine the next task** (`process` or `thread`).<br><br>
(5) `OS` **loads context of the next task from corresponding `TCB` or `PCB`**. The values are applied to `CPU register` (eg. `PC (Program Counter)`, `Stack Pointer`) - `Register` is a place to store information for processing tasks from the `CPU`<br><br>
(6) (Optional) If `Context Switch` involves updates in `memory address`, corresponding updates in `memory management structure` is required. (eg. when switching between `processes` with separate address spaces)<br><br>
(7) Resume execution<br><br>

- `Context Switching` between `processes` could require additional memory management and also **trigger `Cache Pollution` (`Cache memory` is a shared area between `processes`, but `cache data` from previous `process` is less likely to be used in the new `process`)**.


## 6. Concurrency (동시성) vs Parallelism (병렬성)
- **`Concurrency (동시성)`**
    - **Looks as if multiple programs are running at once, but is in fact handling multiple processes with `Context Switching`** (so two tasks cannot be processed at the same moment).
    - can be found at `Multi-Programming` with Single Core.

- **`Parallelism (병렬성)`**
    - Actually running multiple tasks at the same moment.
    - Usually requires CPU with multiple core.
    - `Parallelism` could be achieved even with single core `CPU` if the commands could run all at once (A core of CPU has multiple pipelines. If multiple tasks have no relevance with each other `Parallelism` could be achieved - eg. use different parts of CPU)


## 7. Critical Section (임계영역)
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
            - `Lock` and `Unlock` method could be improved to enable simultaneous lock for `Reading` but only allow single lock for `Writing`.

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



## 8. CPU Scheduling
- **`First Come First Served`**
    - Execute and finish tasks one by one in the requested order.

- **`Round Robin`**
    - Similar to `First-Come-First-Served` but limit the amount of time that the task can hold the `CPU`.
    - If the given time has passed but the task is not finished, the task moves to the back of the `queue`.

- **`Priority`**
    - Set priorities to tasks.
    - Could be improved by increasing priority of task which waited in the queue for a long time.


## 9. Memory Hierarchy
- `Register` > `Cache` > `Main Memory (RAM)` > `Electronic Disk` > `Magnetic Disk` > `Optical Disk` > `Magnetic Tapes`
- The program has to go on `Main Memory` to be executed.
- The memory starting from `Main Memory` are volatile.



## 10. Memory Allocation
- Types of `Addresses`:
    - **`Physical Address`**: **Physical address inside the `Main Memory`**.
    - **`Logical Address`**: Address **created by `CPU` per each `process`, which starts from 0. Used inside a `process`**.

- `Address Binding`: Deciding where to allocate the `Physical Memory Address` of a `process`.
    - `Compile Time`: Decided during compile time.
    - `Load Time`: Decided when the `process` is loaded on to the memory
    - `Execution Time`: Decide on `Runtime` (and also could change during `runtime`). The `CPU` uses `address mapping table` when referring to the `address`.

- Allocating memory for a `process`:
    - **`Contiguous Allocation (연속적 할당)`**
        - Method of allocating `contiguous memory block` to a `process`.
        - `Block`: Small pieces of `memory` with fixed-size.
        - `Hole`: Empty `Block(s)` where a `process` could be assigned.
        - **`Partition`**: **Memory Block(s) consisting of one `process`** (as long as the system is not following `Multi-Partition Alloction`)
        - **`Fragmentation (단편화)`**: Ocurrence of small `memory blocks` that cannot be used.
        - **`Fixed-Partition`** (Static Contiguous Allocation)
            - The system gets divided into **multiple fixed-sized partitions**.
            - Number of allocatable `processes` will be decided by the fixed partition size.
            - **`Internal Fragmentation`**: Leftover blocks when the `partition size` is bigger than what the process actually needs.
            - **`External Fragmentation`**: **Enough memory is remaining but cannot allocate a `process` because the blocks are not continguous**.
                - `External Fragmentation` can be resolved by `Compaction (압축)`** which is **gathering all `partitions` into one side without space, but is inefficient due to overhead**.

        - **`Variable-Partition`** (Dynamic Contiguous Allocation)
            - The **size of each `partition` will be decided by the size of the `process`**.
            - **Only has `External Fragmentation issue`**
        
        - **Pros** of `Contiguous Memory Allocation`:
            - Easy to implement
            - **Faster memory access speed**
        
        - **Cons** of `Contiguous Memory Allocation`:
            - **Fragmentation issue** (`Internal Fragmentation`, `External Fragmentation`)

    - **`Non-contiguous Allocation (비연속적 할당)`**
        - Method of allocating `uncontiguous physical memory block` to a `process`. (all the different parts of the `process` will be allocated to different places in the `memory`)
        - With `Non-contiguous memory allocation`, the **`OS` needs to maintain a `table` inside the `main memory` per each `process` which consist of the `base address` of each `blocks` the `process` acquired** (This is why `memory execution` is slower with `Non-contiguous allocation` since address translation based on this `table` is required).

        - **`Paging`**
            - **Divide the `Main memory` and `Process` by same unit (defined by hardware)**.
            - **`Page`**: **Divided part of `process`** in the secondary memory
            - **`Frame`**: **Divided part of `Main Memory`**.
            - **`External Fragmentation` doesn't happen**, but because `frames` have fixed size, **`internal fragmentation` can still happen**.

        - **Segmentation**
            - **Divide process into `segments` with different sizes which is a logical unit** (eg. `Code`, `Data`, `Stack`, `Heap`)
            - **`Segmentation Table` should have `limit` to prevent accessing unwanted area** because each `segments` has different size. (but seems like people just call it `Page Table` for `Segmentation` as well)
            - **`External Fragmentation` can happen because as processes are loaded, it could be difficult to find a place to assign a large block**.

        - **Pros** of `Non-Contiguous Memory Allocation`:
            - More **efficient way of memory** usage (**less waste** compared to `Contiguous Memory Allocation`)

        - **Cons** of `Non-Contiguous Memory Allocation`:
            - **Slow memory access due to `memory translation`** process


## 11. Virtual Memory
- `Virtual Memory` is a memory management technique where **secondary memory can be used as if it's part of the `Main Memory`.
- `Virtual Memory` maps `Non-Contiguous physical memory` to `Contiguous Logical Memory` for each process (translated during `runtime`).
- `Virtual Memory` is implemented using `Demand Paging` or `Demand Segmentation` (loading the corresponding unit to the main memory on demand).

- **`Swapping`**: **Moving the `process` between the `disk` and the `Main Memory`**.
    - `Swap In`: Load the `process` into `Main Memory`
    - `Swap Out`: Move the `process` from the `Main Memory` to the `disk`.
    - If the address of the `process` was binded during `Compile Time` or `Load Time`, the `process` has to move to consistent location inside `Main Memory`. **If the address of binded during `Execution Time`, the `process` can move to any address**.

- **`TLB (Translation Lookaside Buffer)`**: **Cache buffer containing `<Page number, Frame Number>` for frequently used pages** (`Frame number` will be used with `page offset` to calculate the `physical address`).

- Steps of memory access in `Virtual Memory`:<br>
(1) **`CPU` looks through `TLB`**.<br><br>
(2) If the `page` is found in `TLB` (**`TLB hit`**), the `CPU` will use `frame number` to get the `physical address`.<br><br>
(3) If the `page` is not found in `TLB` (**`TLB miss`**), **the `CPU` would go to the `Page Table`** to look for the `page number`.<br><br>
(4) **If the `frame` is not on the `main memory` (`Page Fault`), `interrupt` will be thrown and `OS` will take control**.<br><br>
(5) **`OS` will `swap-in` the `frame`** from the secondary memory (If the `main memory` is full, **`page replacement algorithm`** will be called to make space). **`Page Table` or `TLB` will be updated** accordingly. Once everything is done, **`signal` will be sent to the `CPU`**.<br><br>
(6) The `CPU` will take control again and proceed with the blocked `process`.<br><br>

- **Advantages of `Virtual Memory`**:
    - **More processes can be maintained on the `main memory` at once (good for `Multi-Tasking`)**
    - **Process can be bigger than the size of `Main Memory`**
    - The memory that a program consumes is split into `main memory` and `secondary memory`, and only required data would go up to the `main memory`. This increases security


## 12. DMA (Direct Memory Access)
- Feature of allowing certain `hardware systems` to access the `main memory` without intervention of `CPU`.
- Without `DMA`, `CPU` is fully occupied during `I/O` data transfer.
- With `DMA`, `CPU` would initiate the transfer, do something else, and receives `interrupt` at the end.
- `DMA` is not only used to handle `I/O` data, but also to move data within `memory`.