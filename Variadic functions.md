[[Printf Project.base]]
###### What are variadic functions and how do they work ?

A variadic function is a function that can take a variable number of arguments.
For exemple:
```c
printf("Hello\n");
printf("Number: %d\n", 42);
printf("%s %d %f\n", "Values:", 10, 3.14);
```
Even if they have different number of arguments all of these are still valid.

###### How does it works ?

To be able to use this, C provides a set of macros in ``<stdarg.h>`` that allows a function to access extra arguments beyond the fixed ones.

These macro are:
- ``va_list`` -a special type used to store the list of arguments.
- ``va_start(list, last_fixed_arg)`` - initializes the list.
- ``va_arg(list, type)`` - retrieves the next argument of the given type.
- ``va_end(list)`` -cleans up when we are done.

###### Exemple:

```c
#include <stdarg.h>
#include <stdio.h>

int sum(int count, ...) // 'count' is the number of variable arguments
{
	val_list args;
	int total = 0;
	int i = 0;
	
	va_start(args, count); // Initialize the argument list
	while(i < count)
	{
		total += va_arg(args, int); // Get next argument (type = int)
		i++;
	}
	va_end(args); // Clean up
	
	return (total);
}

int main(void)
{
	printf("%d\n", sum(3, 1, 2, 4)); // prints 7 (3 is the number of arguments)
}

```

##### 🔍 Key points to remember
1. At least one fixed argument needed. (It acts as a reference point for ``va_start()`` (that's why I passed count in sum (). ))
2.  We must know how to interpret the arguments.
3. Always call ``va_end()`` before the functions returns.

### ⚠️ 6. Common mistakes

- ❌ Forgetting to call `va_end()`.
- ❌ Passing fewer or more arguments than expected.
- ❌ Using the wrong type in `va_arg()` (e.g., reading an `int` as a `double`).
- ❌ Not having at least one fixed argument before `...`.

## 7. Syntax summary

```c
type function_name(fixed_args, ...)
{     
	va_list var_name;
	va_start(var_name, last_fixed_arg);     // use va_arg(var_name, type) to get each argument     
	va_end(var_name); }
```
