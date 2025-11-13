
|                        |                                                                                         |
| ---------------------- | --------------------------------------------------------------------------------------- |
| **Function Name**      | get_next_line                                                                           |
| **Prototype**          | char *get_next_line(int fd)                                                             |
| **Turn in files**      | get_next_line.c, get_next_line_utils.c, <br>get_next_line.h                             |
| **Parameters**         | fd: the [[File descriptor]] to read from                                                |
| **Return value**       | Read line: correct behavior<br>NULL: there is nothing else to read, or an error occured |
| **External functions** | read, malloc, free                                                                      |
| **Description**        | Write a function that returns a line read from a [[File descriptor]]                    |
- Repeated calls  (e.g using a loop) to your ==**get_next_line()**== function should let you read the text file pointed to by the file descriptor, **one line at a time** (it means until the \n character and not the EOF).

- The function should return the line that was read.
  If there is nothing left to read or if an error occurs, it should return **NULL**.

- The function need to work as expected both when reading a file and when reading from the standard input.

- **Please note** that the returned line should include the terminating \n character, except when the end of the file is reached and the file does not end with a \n character.

- Your header file **get_next_line.h** must at least contain the prototype of the get_next_line() function.

- Add all the helper functions you need in the **get_next_line_utils.c** file.

- Because you will have to read files in get_next_line(), add this option to your compiler call: -D BUFFER_SIZE=n
  it will define the buffer size for read().
  The buffer size value will be adjusted by your peer evaluators and the Moulinette to test your code.

- **get_next_line()** exhibits undefined behavior if the file associated with the [[File descriptor]] is modified after the last call, while read() has not yet reached the EOF.

- **get_next_line()** also exhibits undefined behavior when reading a binary file. However, you can implement a logical way to handle this behavior if you want to.

#### Things to do:

What the fuck is [[read()]] and how do I use it ?
	
