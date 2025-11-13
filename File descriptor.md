| **Term**                  | **Summary**                                                                                                                                                                                                                              |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **File Descriptor Table** | A per-process array maintained by the kernel. The file descriptor (the integer) is an index into this array, which maps to the actual _open file description_ stored in the system-wide file table.                                      |
| **File Offset**           | The current position within the open file that the next read or write operation will start from. This is tracked per-FD, which means two FDs pointing to the same file can have different offsets.                                       |
| **inode**                 | The **Index Node** is a structure on the disk that stores all the metadata about a file (permissions, owner, size, physical data block locations) _except_ its filename. The file descriptor links through the File Table to this inode. |

### Name

```
File Descriptor (FD) - The I/O Handle
```

### Synopsis

A non-negative integer that uniquely identifies an open file, socket, pipe, or other input/output resource within a specific process.

### Description

In Unix, the mantra is "Everything is a File." A file descriptor is the abstraction that makes this true.

    It's an Integer Index: From the perspective of your running program (in User Space), the file descriptor is just a small, non-negative integer (like 3, 4, 5, etc.).

The Kernel's Link: This integer is an index into a per-process File Descriptor Table maintained by the operating system's core (the Kernel Space). This table, in turn, points to a system-wide File Table Entry, which holds the real bookkeeping information about the open resource, such as:

    The access mode (read-only, write-only, etc.)

The file offset (the current position for reading or writing)

        A pointer to the actual underlying file's metadata (inode).

### Default File Descriptors

Every process automatically inherits three file descriptors from its parent (usually the shell) when it starts. You've likely been using them without even knowing!

|**Descriptor**|**Name**|**C Macro**|**Default Connection**|**Purpose**|
|---|---|---|---|---|
|**0**|Standard Input|`STDIN_FILENO`|Keyboard/Input stream|Data for the program to _read_.|
|**1**|Standard Output|`STDOUT_FILENO`|Terminal screen|Normal output data for the program to _write_.|
|**2**|Standard Error|`STDERR_FILENO`|Terminal screen|Error/diagnostic messages for the program to _write_.|
When you use a system call like open(), the kernel returns the lowest available non-negative integer as the new file descriptor. This is why the first file you explicitly open usually gets FD 3 (since 0, 1, and 2 are already taken).

https://www.youtube.com/watch?v=rW_NV6rf0rM
