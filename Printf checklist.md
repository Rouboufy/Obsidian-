[[Printf Project.base]]
[[Variadic functions]]
**Status:** In Progress

##  Necessary implementation

- [x] %c Prints a single character.
- [x] %s Prints a string (as defined by the common C convention).
- [ ] %p The void * pointer argument has to be printed in hexadecimal format.
- [ ] %d Print a decimal (base 10) number.
- [ ] %i Prints a integer in base 10.
- [ ] %u Prints an unsigned decimal (base 10) number.
- [ ] %x Prints a number in hexadecimal (base 16) lowercase format.
- [ ] %X Prints a number in hexadecimal (base 16) uppercase format.
- [ ] %_% Prints a percent sign.

## 🎨 Name checking and norminette checks.

- [ ] Norm compliant ?
- [ ] Pdf compliant ?

## 🐛 Bugs & Issues

- [ ] None so far


### What does the printf function does ?

First things first what is the prototype of the ``printf()`` function ?
	And what are it's necessary parts ?

```c
#include <stdarg.h>

int ft_printf(const char *format, ...)
{
}
```
##### Little explanation of the prototype:
Why the return value is an int ? 
	it's simply because it's simpler to spots mistakes or error like that. (it exit with 0 if everything went smoothly, and in case of an error it exits with  > 0).

What is the pointer format part for ?
	The format string serves 2 main purpose:
	- 1: **Literal text** : Any character in the string that are not format specifier are printed to the output exactly how it is, without any modifications.
	-  2: **Format specifier** : This strings contains special sequences, beginning with a percent sign (``%``), called **format specifier**, this placeholders tells ``printf()``:
		- **What type of data** (like an integer, a floating-point number, a single character, or another string) to expect from the subsequent arguments.
		- **How to format** that data for output (e.g., how many decimal places to show, the minimum width of the output field, left or right justification, etc.).

What is the ... part stands for ?
	The ... part is linked to the [[Variadic functions]], it let us enter a variable number of arguments.
	                             |--->read if you want to know more. 



### ==Step 1:==

I'm not quiet sure yet but I guess that the first thing to do is to print in the first argument in the standard output, without doing anything.

Something like (prototype test)
```c
#include <unistd.h>

int ft_printf(const char *format){

	while (*format)
	{
		write(1, &format, 1);
		format++;
	}
	return (0);

}
```
|
|----> by doing this I print everything to the ``STDOUT`` (but this isn't correct because by doing this             ignore totally the **format specifier part** which is no good).
|
|----> Which bring us to the step 2.

### ==Step 2:==

1





