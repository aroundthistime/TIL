# OS (Operating System)

### 1. OS
- Program which is loaded by boot program, and manages all other application programs in a computer.
- `OS` will be in the middle of communication between hardware and the application program.


### 2. Kernel
- `Kernel` is the core part of `OS`, which will be on the memory of computer by boot program (because `OS` also is a heavy program, it cannot go on the memory entirely. Only the `Kernel` is brought, and the others will be called if necessary)
- `Kernel` will be an interface between `hardware` and the `software`:
    - **`Resource Management`**: Manage **system resources (eg. `CPU`, `Memory`, `Disk space`)** - includes **(de)allocation of resources while setting priorities between processes**.
    - **`Process Management`**: Manage **creation, termination, suspension, scheduling, and context switching of processes** and also communication between them.
    - **`System Call Management`**: Handles system calls to allow applications access to hardware resources (so basically an **interface between the system and the hardware**) - **`System Call` is created when a program wants to send request to the `Kernel` (OS)**
    - **`System Security`**: Manages **access control and permissions** to ensure the security of the system.


### 3. Process vs Thread
- **`Process`**
    - An **instance of program executed by one or more threads**.
    - Gets **allocated a separate resource by `Operating System`** so that processes are less likely to interfere with each other.
    - `Process` has **at least one thread (`main thread`)**.
    - In some cases (eg. Program which runs by `multi-process`), **multiple `processes` need to interact with each other**s. This is done by `IPC (Inter-process Communication)`. There could be 2 major ways of enabling communication between processes:
        - **`Message Passing`**: Pass messages between processes via `Kernel` (has overhead)
        - **`Shared Memory`**: Sharing memory between processes (no need to call system call but has several issues - `Producer-Consumer Issue`, `Critical Section`, ..)

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