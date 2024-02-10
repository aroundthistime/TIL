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
    - **`System Security`**: Manages **access control and permissions** to ensure the security of the system.