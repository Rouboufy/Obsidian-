==Necessary knowledge:==
- [[Fork()]]
- [[mmap()]]
- [[execve()]]
- [[wait()]]
- [[dup2()]]
- [[perror()]]
- [[strtok()]]

### Phase 1 setup & Foundation

#### ==Objective==:

Get a minimal shell that:
- Reads a command line from the stdin
- Parses it into program + arguments
- Executes it
- Waits for it to finish
- Loop forever

--------------------------------------------------------------
##### Project setup:


```c
minishell
|
|----> Makefile
|----> main.c

```
**The binary will be called minishell***

#### The included libraries

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>
```

**Rule**: no fancy readline or ncurses yet. I have to use getline() or your own implementation with read().

### Minimal loop pseudo code

```c
loop forever:
	display prompt("$> ")
	read line from stdin
	if line is empty: continue
	split line into argv[]
	create a new process
		if child:
			execve(argv[0], argv, envp)
			if execve fails print error
		if parent:
			wait for child to finish
```

Apparently it's important to flush or disable via `setbuf(stdout, NULL);` the stdout before reading stdin but I'm not sure how important it is.

This time i have to use the function getline() to be able to read on the stdin such as in:
```c
size_t len = 0;
char *line = NULL;

while (getline(&line, &len, stdin) != -1) {
	//line contains the data
}

free(line);

```

### Some Recommend Practices for production Code 

When writing production programs that need to handle stdin properly, keep these best practices in mind:

- Use fgets() over gets() for safety
- Validate line length and truncate if too long
- Flush stdout before reading stdin
-  Actively check for EOF rather than assume input
-  Avoid arbitrary limits on line lengths
-  Reset stdin state after errors with fflush(stdin)
- Prefer portable functions like fgets() over POSIX ones
- Disable buffering entirely for piped data
- Read stdin in chunks/loops instead of all at once

Adopting defensive coding practices like these will prevent crashes, overflow issues, and weird pipe behavior in C applications that process stdin.
