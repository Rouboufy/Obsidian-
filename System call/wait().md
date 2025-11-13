### Name 

```
wait, waitpid, waitid - wait for the process to change state
```

### Synopsis

```c
#include <sys/wait.h>

pid_t wait(int *_Nullable wstatus);
pid_t waitpid(pid_t, int *_Nullable wstatus, int options);

int waitid(idtype_t idtype, id_t id, siginfo_t *infop, int options);
                       /* This is the glibc and POSIX interface; see
                          NOTES for information on the raw system call. */

   Feature Test Macro Requirements for glibc (see feature_test_macros(7)):

       waitid():
           Since glibc 2.26:
               _XOPEN_SOURCE >= 500 || _POSIX_C_SOURCE >= 200809L
           glibc 2.25 and earlier:
               _XOPEN_SOURCE
                   || /* Since glibc 2.12: */ _POSIX_C_SOURCE >= 200809L
                   || /* glibc <= 2.19: */ _BSD_SOURCE
```

### Descriptions 

All of these system calls are used to wait for state changes in a child of the calling process, and obtain information about the child whose state has changed. A state change is considered to be:
- The child was terminated;
- The child was stopped by the signal;
- The child was resumed by the signal.
In the case of a terminated child, performing a wait allows the system to release the resources associated with the child / if a wait is not performed, then the terminated child remains in a "zombie" state.

If a child has already changed state, then these calls returns immediately. Otherwise, they block until either a child changes state or a signal handler interrupts the call (assuming that system calls are not automatically restarted using the **SA_RESTART** flag of **sigaction()**). 
A child whose state has changed and which has not yet been waited upon by one of these system calls is termed ==waitable==.

### RETURN VALUE
wait(): on success, returns the process ID of the terminated child; on failure, -1 is returned.

**waitpid()**:  on success, returns the process ID of the child whose state has changed; if WNOHANG was specified and one or more child(ren) specified by pid exist, but have not yet changed state, then 0 is returned.  On failure, -1 is returned.

**waitid():** returns 0 on success or if WNOHANG was specified and no child(ren) specified  by  id  has yet changed state; on failure, -1 is returned.

On failure, each of these calls sets errno to indicate the error.

### ERRORS

**EAGAIN** The PID file descriptor specified in id is nonblocking and the process that it refers to has not terminated.

**ECHILD** (for wait()) The calling process does not have any unwaited-for children.

**ECHILD** (for  waitpid()  or  waitid())  The  process  specified  by pid (waitpid()) or idtype and id (waitid()) does not exist or is not a child of the calling process.  (This  can  happen  for one's  own child if the action for SIGCHLD is set to SIG_IGN.  See also the Linux Notes section about threads.)

**EINTR**  WNOHANG was not set and an unblocked signal or a SIGCHLD was caught; see signal(7).

**EINVAL** The options argument was invalid.

**ESRCH**  (for wait() or waitpid()) pid is equal to INT_MIN.