### Name
```
setpgid, getpgid, setpgrp, getpgrp - set/get process group
```

### Synopsis 
```c
       #include <unistd.h>

       int setpgid(pid_t pid, pid_t pgid);
       pid_t getpgid(pid_t pid);

       pid_t getpgrp(void);                            /* POSIX.1 version */
       [[deprecated]] pid_t getpgrp(pid_t pid);        /* BSD version */

       int setpgrp(void);                              /* System V version */
       [[deprecated]] int setpgrp(pid_t pid, pid_t pgid);  /* BSD version */

   Feature Test Macro Requirements for glibc (see feature_test_macros(7)):

       getpgid():
           _XOPEN_SOURCE >= 500
               || /* Since glibc 2.12: */ _POSIX_C_SOURCE >= 200809L

       setpgrp() (POSIX.1):
           _XOPEN_SOURCE >= 500
               || /* Since glibc 2.19: */ _DEFAULT_SOURCE
               || /* glibc <= 2.19: */ _SVID_SOURCE

       setpgrp() (BSD), getpgrp() (BSD):
           [These are available only before glibc 2.19]
           _BSD_SOURCE &&
               ! (_POSIX_SOURCE || _POSIX_C_SOURCE || _XOPEN_SOURCE
                   || _GNU_SOURCE || _SVID_SOURCE)
```

### Description:

All  of  these  interfaces are available on Linux, and are used for getting and setting the process group ID (PGID) of a process.  The preferred, POSIX.1-specified  ways  of  doing  this  are:  getpgrp(void), for retrieving the calling process's PGID; and setpgid(), for setting a process's PGID.

setpgid()  sets the PGID of the process specified by pid to pgid.  If pid is zero, then the process ID of the calling process is used.  If pgid is zero, then the PGID of the process specified by  pid is  made the same as its process ID.  If setpgid() is used to move a process from one process group to another (as is done by some shells when creating pipelines), both process groups must be part of the same session (see setsid(2) and credentials(7)).  In this case, the pgid specifies an  existing process  group to be joined and the session ID of that group must match the session ID of the joining process.

The POSIX.1 version of getpgrp(), which takes no arguments, returns the PGID of the calling process.

**getpgid()** returns the PGID of the process specified by ==pid==. If ==pid== is zero, the process ID of the calling process is used. (Retrieving the PGID of a process other than the caller is rarely necessary, and the POSIX.1 **getpgrp()** is preferred for that task.)

The System V-style setgrp(), which takes no arguments, is equivalent to ==**setpgid(0, 0).**==

### Return value

On success, **setpgid()** and **setpgrp()** return zero. On error, -1 is returned, and **errno** is set to indicate the error.

The POSIX.1 **getpgrp()** always returns the PGID of the caller.

**getpgid()**, and the BSD-specific **getpgrp()** return a process group on success. On error, -1 is returned, and **errno** is set to indicate the error.


### ERRORS

**EACCES** `An  attempt  was  made  to change the process group ID of one of the children of the calling process and the child had already performed an execve(2) (setpgid(), setpgrp()).`

**EINVAL** `pgid is less than 0 (setpgid(), setpgrp()).`

**EPERM**  `An attempt was made to move a process into a process group in a  different  session,  or  to change  the process group ID of one of the children of the calling process and the child was in a different session, or to change the process group ID of a  session  leader  (setpgid(), setpgrp()).`

**EPERM**  `The target process group does not exist.  (setpgid(), setpgrp()).`

**ESRCH**  `For  getpgid():  pid  does  not  match  any  process.  For setpgid(): pid is not the calling process and not a child of the calling process.`