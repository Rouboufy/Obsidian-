### Name 

```
read - read from a file descriptor
```

### Synopsis

```c
#include <unistd.h>

ssize_t read(int fd, void buf[.count], size_t count;

```

### Description 

**read()** attempts to read up to count bytes from the [[File descriptor]] fd into the buffer starting at buf.

On files that support seeking, the read operation commences at the file offset, and the file offset is incremented by the number of bytes read. If the file offset is at or past the EOF, no bytes are read, and **read()** returns zero.

If count is zero, **read()** may detect the errors described below. In the absence of any errors, or if **read()** does not check for errors, a **read()**  with a count of 0 returns zero and has no other effects.

### RETURN VALUE

On success, the number of bytes read is returned (zero indicates end of file), and the  file  position  is  advanced by this number.  It is not an error if this number is smaller than the number of bytes requested; this may happen for example because fewer bytes are actually available  right  now (maybe  because we were close to end-of-file, or because we are reading from a pipe, or from a terminal), or because read() was interrupted by a signal.

On error, -1 is returned, and errno is set to indicate the error.  In this case, it is left unspecified whether the file position (if any) changes.