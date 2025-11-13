### Name

```
dup, dup2, dup3 - duplicate a file descriptor
```

### Synopsis

```
#include <unistd.h>

int dup(int oldfd);
int dup2(int oldfd, int newfd);

#define _GNU_SOURCE
#include <fcntl.h>
#include <unistd.h>

int dup3(int oldfd, int newfd, int flags);
```

### Description 
The dup2() system call performs the same task as [[dup()]], but instead of using the lowest-numbered unused [[File descriptor]], it uses  the [[File descriptor]] number specified in ==newfd==.  In other words, the file descriptor ==newfd== is adjusted so that it now refers to the same open [[File descriptor]] as ==oldfd==.

If the [[File descriptor]] ==newfd== was previously open, it is closed before being reused; the close is performed silently (i.e, any errors during the close are not reported by dup2()).

The steps of closing and reusing the file descriptor ==newfd== are performed atomically. **This is important**, because trying to implement equivalent functionality using [[close()]] and [[dup()]] would be subject to race conditions, whereby ==newfd== might be reused between the two steps. Such reuse could happen because the main program is interrupted by a signal handler that allocates a file descriptor, or because a parallel thread allocates a file descriptor.

Note the following points:

- If ==oldfd== is not a valid file descriptor, then the call fails, and ==newfd== is not closed.
- If ==oldfd== is a valid file descriptor, and ==newfd== has same value as ==oldfd==, then dup2() does nothing, and returns ==newfd==.

### Return value 

On success, these system calls return the new file descriptor. On error, -1 is returned, and ==errno== is set to indicate the error.

### ERRORS 

**EBADF**  `oldfd isn't an open file descriptor.`

**EBADF**  `newfd is out of the allowed range for file descriptors (see the discussion of  RLIMIT_NOFILE in getrlimit(2)).`

**EBUSY** `(Linux  only)  This may be returned by dup2() or dup3() during a race condition with open(2) and dup().`

**EINTR**  `The dup2() or dup3() call was interrupted by a signal; see signal(7).`

**EINVAL** `(dup3()) flags contain an invalid value.`

**EINVAL** `(dup3()) oldfd was equal to newfd.`

**EMFILE** `The per-process limit on the number of open file descriptors has been reached (see the  discussion of RLIMIT_NOFILE in getrlimit(2)).`
