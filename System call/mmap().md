
### Name
```
 mmap, munmap - map or unmap files or devices into memory
```

### Synopsis 

```c
#include <sys/mman.h>

void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);

int munmap(void *addr, size_t length);
```

### Description
#### what is mapping ? 

Mapping means creating a direct correspondence between two things. In the case of mmap(), a correspondence is created between:
- **A virtual memory area** (addresses in your program)
- **A content** (either a file on disk, or memory)

Whiteout mmap(), traditional method: 
```c
// 1. Open the file
int fd = open("data.txt", O_RDONLY);

// 2. Allocate buffer in memory
char *buffer = malloc(1000);

// 3. Copy the data from the file to the buffer
read(fd, buffer, 1000);

// 4. Use the buffer
printf("%s", buffer);

// 2 copy of the data one on the disk and one in RAM
```
With mmap():
```c
// 1. Open the file
int fd = open("data.txt", O_RDONLY);
// 2. MAP directly the file in memory 
char *data = mmap(NULL, 1000, PROT_READ, MAP_PRIVATE, fd, 0); 
// 3. Use as if it was already in RAM
printf("%s", data); 
// Only one logic copy in memory.
```

```c
Sans mmap():
┌─────────────┐                    ┌──────────────┐
│  File       │  --read()-->       │   Buffer     │
│ on the disk │  (copy)            │   in RAM     │
└─────────────┘                    └──────────────┘
                                    The program use this


Avec mmap():
┌─────────────┐
│  File       │ ←──────┐
│ on the disk │        │ mapping (correspondance)
└─────────────┘        │
                       ↓
                  ┌──────────────┐
                  │ Memory       │
                  │ address in   │
                  │ your program │
                  └──────────────┘

```

What happens when you mmap() ?
- The OS allocate a memory address within your program.
- It creates a equivalency table so has "address 0x1000 correspond to the 0 octet of the file."
- When we access for example data[100] the process will look the table and grab the 100th octet of the file.
- The system automatically charge the needed part of the file (per page of 4Ko).

```c
#include <sys/mman.h>

void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);

```

**Parameters:**

- **addr**: suggested address (usually NULL to let the system choose)
- **length**: size of the area to map (in bytes)
- **prot**: memory protections (read/write/execute)
	- ``PROT_READ``: read access
	- ``PROT_WRITE``: write access
	- ``PROT_EXEC``: execute access
	- ``PROT_NONE``: no access
- **flags**: mapping options
	- ``MAP_SHARED``: changes are shared with other processes and the file
	- ``MAP_PRIVATE``: changes are private (copy-on-write)
	- ``MAP_ANONYMOUS``: no file, just memory
- **fd**: file descriptor to map (-1 if MAP_ANONYMOUS)
- **offset**: position in the file (must be page aligned)
#### Return value
Pointer to the mapped area or ``MAP_FAILED`` in case of an error.