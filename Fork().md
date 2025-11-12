summary of used term:
[[mmap()]]
[[setpgid()]]


### Name
```
fork - creat a child process
```

### Synopsis
```c
#include <unistd.h>
pid_t fork(void)
```
### Description

**fork()** creates a new process by duplicating the calling process. The new process is referred to as the ==child== process. The calling process is referred to as the ==parent== process.

The ==child== process and the parent process run in separate memory spaces. At the time of fork() both memory spaces have the same content. Memory writes, file mappings (==[[mmap()]]==), and unmappings (==munmap()==) performed by one of the process do not affect the other.

**==The child process is an exact duplicate of the parent process except for the following points:==**

- The ==child== has is own unique process ID, and this PID does not match the ID of any existing process group ([[setpgid()]] or session).

- The child's parent process ID is the same as the parent's process ID.

- The child process's set of pending signals is empty. (The parent's pending signals are not inherited).

- The child does not inherit any record locks set by the parent (see [[fcntl()]]).

- The child's resource utilization and CPU time counters are reset to zero.

 - Any memory locks established by the parent via [[mlock()]] or [[mlockall()]] are not inherited by the child.

- Process interval timers are reset in the child.

- The child does not inherit the parent's process-wide-semaphore-adjustments (made via [[semop()]]).

### Return Value

Upon successful completion, fork() returns 0 in the child process and returns the process ID of the child in the parent process.

On failure, fork() returns -1 in the parent, no child process is created, and errno is set to indicate the error.
 
### Errors

    EAGAIN: The system-imposed limit on the total number of processes under a user ID has been reached, or the system-wide limit on the total number of processes has been reached. (Basically, the system is all out of processes—a very rude error, if you ask me.)

    ENOMEM: A kernel data structure (like the process table) could not be allocated, or there was insufficient swap space for the new process's private data pages.