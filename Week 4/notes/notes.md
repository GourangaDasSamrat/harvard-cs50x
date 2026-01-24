# CS50 Lecture 4 - Memory & Pointers

## Personal Learning Notes

---

## 🎯 Key Takeaways

- **Memory addresses are fundamental** - Every variable lives at a specific location
- **Pointers store addresses** - They're variables that point to other variables
- **Strings are character pointers** - `string` is just `char *` in disguise
- **Memory management is YOUR job** - `malloc()` to allocate, `free()` to release
- **Pass by value vs reference** - Functions get copies unless you pass addresses
- **Buffer overflows are dangerous** - Always allocate enough memory!

---

## 🔢 Hexadecimal (Base-16)

### Why Hexadecimal?

**Hexadecimal** represents information more succinctly than binary or decimal

**The 16 digits**:

```
0 1 2 3 4 5 6 7 8 9 A B C D E F
```

Where:

- `A` = 10
- `B` = 11
- `C` = 12
- `D` = 13
- `E` = 14
- `F` = 15

### Counting in Hexadecimal

```
Decimal | Hexadecimal
--------|------------
   0    |     00
   1    |     01
   9    |     09
  10    |     0A
  15    |     0F
  16    |     10
 255    |     FF
```

### How to Calculate

**Each position is a power of 16**:

```
Position:  16¹   16⁰
Value:      16    1
```

**Example: FF in decimal**

```
F × 16 = 15 × 16 = 240
F × 1  = 15 × 1  = 15
                  ----
                   255
```

### The `0x` Prefix

**Convention**: Hexadecimal numbers are prefixed with `0x`

```
0x00  = 0
0x0A  = 10
0x10  = 16
0xFF  = 255
```

**Why?** Distinguishes hex from decimal

- `10` could be ten (decimal) or sixteen (hex)
- `0x10` is clearly sixteen

### Hexadecimal in RGB Colors

**RGB values** range from 0-255 (00-FF in hex)

```
Color        | R    G    B  | Hex
-------------|--------------|--------
Red          | 255  0    0  | #FF0000
Green        |  0  255   0  | #00FF00
Blue         |  0   0  255  | #0000FF
White        | 255 255 255  | #FFFFFF
Black        |  0   0    0  | #000000
```

**Example from Photoshop**: `255` displayed as `FF`

---

## 💾 Memory Fundamentals

### Memory as Grid of Bytes

Think of computer memory as a giant array of bytes:

```
Address  | Value
---------|-------
  0x00   |  ...
  0x01   |  ...
  0x02   |  ...
  0x03   |  ...
  ...    |  ...
```

**Each byte has an address** - that's how we find data!

### Variables in Memory

```c
int n = 50;
```

**What happens?**

1. Computer allocates memory for an `int` (4 bytes)
2. Stores value `50` in that memory
3. Variable `n` refers to that location

**Visual representation**:

```
Variable: n
Value:    50
Address:  0x12345678 (example)
```

---

## 👉 Pointers - The Core Concept

### What is a Pointer?

**Pointer**: A variable that stores a memory address

**Think of it as**: An arrow pointing to where data lives

### The Two Essential Operators

#### `&` - Address-of Operator

**Gets the address** of a variable

```c
int n = 50;
printf("%p\n", &n);  // Prints address of n
```

**Output**: `0x7ffe5367e044` (or similar)

**`%p`** = format code for pointer (address)

#### `*` - Dereference Operator

**Has two meanings** depending on context:

**1. Declaring a pointer**:

```c
int *p;  // p is a pointer to an integer
```

**2. Dereferencing (getting value at address)**:

```c
int n = 50;
int *p = &n;
printf("%i\n", *p);  // Prints 50 (value at address p)
```

### Complete Pointer Example

```c
#include <stdio.h>

int main(void)
{
    int n = 50;        // Create integer
    int *p = &n;       // p stores address of n

    printf("%i\n", n);     // Prints: 50
    printf("%p\n", &n);    // Prints: 0x... (address)
    printf("%p\n", p);     // Prints: 0x... (same address)
    printf("%i\n", *p);    // Prints: 50 (value at p)
}
```

**Breaking it down**:

- `n` = the value (50)
- `&n` = address where n is stored
- `p` = variable holding that address
- `*p` = value at the address stored in p

### Visualizing Pointers

**In memory**:

```
Address    | Variable | Value
-----------|----------|-------
0x123      |    n     |  50
0x456      |    p     | 0x123 (points to n)
```

**Conceptual diagram**:

```
    p           n
[0x123] -----> [50]
```

The pointer `p` contains the address of `n`, effectively pointing to it!

---

## 📝 Strings Revealed

### The Truth About Strings

**What we've been using**:

```c
string s = "HI!";
```

**What it REALLY is**:

```c
char *s = "HI!";
```

**`string` is just a convenient typedef** for `char *`!

### How CS50 Defines String

```c
typedef char *string;
```

This is in the `cs50.h` library - just a training wheel!

### Strings in Memory

**A string is**:

- An array of characters
- Ending with `\0` (null terminator)
- The variable stores the **address of the first character**

**Example**: `"HI!"`

```
Address | Value
--------|------
0x123   |  'H'
0x124   |  'I'
0x125   |  '!'
0x126   | '\0'  ← Null terminator (marks end)
```

**The variable `s`**:

```
s = 0x123  (address of 'H')
```

### Accessing String Characters

```c
char *s = "HI!";

// These are equivalent:
printf("%c\n", s[0]);      // H
printf("%c\n", *s);        // H

printf("%c\n", s[1]);      // I
printf("%c\n", *(s + 1));  // I

printf("%c\n", s[2]);      // !
printf("%c\n", *(s + 2));  // !
```

**Array notation** (`s[1]`) is syntactic sugar for **pointer arithmetic** (`*(s + 1)`)!

### Printing String Addresses

```c
char *s = "HI!";

printf("%p\n", s);      // Address of first char
printf("%p\n", &s[0]);  // Same address
printf("%p\n", &s[1]);  // Next address
printf("%p\n", &s[2]);  // Next address
printf("%p\n", &s[3]);  // Address of '\0'
```

**Addresses are consecutive** - characters are stored side-by-side!

---

## 🔤 Pointer Arithmetic

### Moving Through Memory

**Pointer arithmetic** lets you navigate memory

```c
char *s = "HI!";

printf("%s\n", s);      // HI!
printf("%s\n", s + 1);  // I!
printf("%s\n", s + 2);  // !
```

**How it works**:

- `s` points to 'H'
- `s + 1` points to 'I' (next byte)
- `s + 2` points to '!' (two bytes over)

**Printing from that point** shows the rest of the string!

---

## 🔍 String Comparison - The Problem

### Why `==` Fails for Strings

```c
char *s = get_string("s: ");  // User types "HI!"
char *t = get_string("t: ");  // User types "HI!"

if (s == t)  // This compares ADDRESSES, not content!
{
    printf("Same\n");
}
else
{
    printf("Different\n");  // Always prints this!
}
```

**Why it's always different**:

```
Memory:
Address | Data
--------|------
0x123   | "HI!" ← s points here
0x456   | "HI!" ← t points here

0x123 ≠ 0x456, so s ≠ t
```

**Even though the content is the same**, they're at different addresses!

### The Solution: `strcmp()`

```c
#include <string.h>

char *s = get_string("s: ");
char *t = get_string("t: ");

if (strcmp(s, t) == 0)  // strcmp returns 0 if equal!
{
    printf("Same\n");
}
else
{
    printf("Different\n");
}
```

**How `strcmp()` works**:

- Returns `0` if strings are identical
- Returns negative if s < t (alphabetically)
- Returns positive if s > t (alphabetically)

**Always use `strcmp()` for string comparison!**

---

## 📋 Copying Strings - The Challenge

### The Wrong Way

```c
string s = get_string("s: ");
string t = s;  // WRONG! Just copies the address!

t[0] = toupper(t[0]);  // Capitalizes first letter

printf("s: %s\n", s);  // Prints: HI!
printf("t: %s\n", t);  // Prints: HI!
```

**Problem**: Both changed because they point to the same memory!

```
Memory:
     s ──┐
         ├──> "hi!"
     t ──┘
```

### The Right Way - Using `malloc()`

**`malloc()`** = memory allocate

**Syntax**: `malloc(size_in_bytes)`

```c
#include <stdlib.h>
#include <string.h>

char *s = get_string("s: ");

// Allocate memory for copy (length + 1 for '\0')
char *t = malloc(strlen(s) + 1);

// Copy the string
strcpy(t, s);

// Now we can modify t without affecting s
t[0] = toupper(t[0]);

printf("s: %s\n", s);  // Prints: hi!
printf("t: %s\n", t);  // Prints: Hi!

// IMPORTANT: Free the memory when done!
free(t);
```

**Now they're separate**:

```
Memory:
s ──> "hi!"
t ──> "Hi!"  (separate copy)
```

### Manual Copy (Understanding the Process)

```c
char *s = get_string("s: ");
char *t = malloc(strlen(s) + 1);

// Copy character by character
for (int i = 0, n = strlen(s); i <= n; i++)
{
    t[i] = s[i];  // Includes '\0' at the end
}

t[0] = toupper(t[0]);
```

**Why `i <= n`?** To copy the null terminator `\0`!

### Using `strcpy()`

**Built-in function** that does the copying for you:

```c
char *s = get_string("s: ");
char *t = malloc(strlen(s) + 1);

strcpy(t, s);  // Copies s into t

t[0] = toupper(t[0]);

free(t);  // Always free malloc'd memory!
```

---

## ⚠️ Memory Management

### The `malloc()` and `free()` Duo

**`malloc()`**: Request memory from the system
**`free()`**: Return memory to the system

**Golden rule**: Every `malloc()` needs a `free()`!

```c
// Request memory
char *s = malloc(100);

// Use the memory
strcpy(s, "Hello");

// Return the memory when done
free(s);
```

### Checking for `NULL`

**`malloc()` returns `NULL` if it fails** (out of memory)

```c
char *s = malloc(100);
if (s == NULL)
{
    printf("Out of memory!\n");
    return 1;  // Exit with error
}

// Safe to use s now
strcpy(s, "Hello");

free(s);
```

**Good practice**: Always check for `NULL`!

### Memory Leak Example

```c
// BAD - Memory leak!
void create_strings(void)
{
    char *s = malloc(100);
    strcpy(s, "Hello");
    // No free()! Memory is lost!
}
```

**Every time this runs**, 100 bytes are lost until program ends!

### Correct Pattern

```c
char *s = malloc(size);
if (s == NULL)
{
    return 1;  // Handle error
}

// Use s...

free(s);  // Clean up!
return 0;
```

---

## 🔧 Valgrind - Memory Checker

### What is Valgrind?

**Valgrind** detects memory-related errors:

- Memory leaks (forgot to `free()`)
- Invalid memory access (reading/writing wrong locations)
- Uninitialized values

### Using Valgrind

```bash
# Compile your program
make memory

# Run with valgrind
valgrind ./memory
```

### Example: Finding Leaks

**Bad code**:

```c
int main(void)
{
    int *x = malloc(3 * sizeof(int));
    x[0] = 72;
    x[1] = 73;
    x[2] = 33;
    // No free(x)!
}
```

**Valgrind output**:

```
==1234== LEAK SUMMARY:
==1234==    definitely lost: 12 bytes
```

**Fixed code**:

```c
int main(void)
{
    int *x = malloc(3 * sizeof(int));
    x[0] = 72;
    x[1] = 73;
    x[2] = 33;
    free(x);  // Fixed!
}
```

**Valgrind now reports**: `All heap blocks were freed -- no leaks are possible`

---

## 🗑️ Garbage Values

### Uninitialized Memory

**When you allocate memory**, it's not automatically zeroed out!

```c
int scores[1024];

for (int i = 0; i < 1024; i++)
{
    printf("%i\n", scores[i]);  // Prints garbage!
}
```

**Output might be**:

```
0
0
1847263
98234
0
...
```

**Why?** Memory was previously used, still has old values!

### Solution: Initialize

```c
int scores[1024];

// Initialize to zero
for (int i = 0; i < 1024; i++)
{
    scores[i] = 0;
}
```

**Or use initializer**:

```c
int scores[1024] = {0};  // All elements = 0
```

---

## 🔄 Swapping Values

### The Problem: Pass by Value

```c
void swap(int a, int b)
{
    int tmp = a;
    a = b;
    b = tmp;
}

int main(void)
{
    int x = 1;
    int y = 2;

    printf("x=%i, y=%i\n", x, y);  // x=1, y=2
    swap(x, y);
    printf("x=%i, y=%i\n", x, y);  // Still x=1, y=2!
}
```

**Why doesn't it work?**

**Functions get COPIES of values**, not the originals!

**Visual**:

```
main's memory:        swap's memory:
x = 1                 a = 1 (copy of x)
y = 2                 b = 2 (copy of y)
```

Changes to `a` and `b` don't affect `x` and `y`!

### The Solution: Pass by Reference

**Pass addresses** instead of values:

```c
void swap(int *a, int *b)  // Takes pointers
{
    int tmp = *a;  // Get value at a
    *a = *b;       // Set value at a
    *b = tmp;      // Set value at b
}

int main(void)
{
    int x = 1;
    int y = 2;

    printf("x=%i, y=%i\n", x, y);  // x=1, y=2
    swap(&x, &y);                   // Pass addresses!
    printf("x=%i, y=%i\n", x, y);  // x=2, y=1 ✓
}
```

**Now it works!**

**Visual**:

```
main's memory:        swap's memory:
x = 1  ←──────────── a (points to x)
y = 2  ←──────────── b (points to y)
```

Function can modify the original values!

---

## 🌊 Buffer Overflow

### Stack Overflow

**Stack**: Memory for function calls

**Stack overflow**: Too many function calls (usually infinite recursion)

```c
void recurse(void)
{
    recurse();  // Calls itself forever!
}
// Eventually: "Segmentation fault"
```

### Heap Overflow

**Heap**: Memory from `malloc()`

**Heap overflow**: Writing beyond allocated memory

```c
int *x = malloc(3 * sizeof(int));  // Space for 3 ints

x[0] = 72;  // OK
x[1] = 73;  // OK
x[2] = 33;  // OK
x[3] = 100; // ERROR! Out of bounds!
```

**Result**: Undefined behavior (crash, corruption, security vulnerability)

### Buffer Overflow

**Generic term** for writing beyond memory boundaries

**Why it's dangerous**:

- Can crash programs
- Can corrupt data
- **Can be exploited** by attackers

**Prevention**:

- Always allocate enough memory
- Check array bounds
- Use tools like Valgrind

---

## ⌨️ `scanf()` - Getting Input

### Basic Integer Input

```c
int n;
printf("n: ");
scanf("%i", &n);  // Note the &!
printf("n: %i\n", n);
```

**Why `&n`?** `scanf()` needs the **address** to store the value!

### The String Problem

```c
// DANGEROUS!
char s[4];  // Only 4 bytes!
printf("s: ");
scanf("%s", s);  // What if user types more than 3 chars?
printf("s: %s\n", s);
```

**Problems**:

1. No `&` needed (array name is already an address)
2. **Buffer overflow** if input is too long!
3. Hard-coded size (inflexible)

### With `malloc()`

```c
char *s = malloc(100);  // Allocate 100 bytes
if (s == NULL)
{
    return 1;
}

printf("s: ");
scanf("%s", s);
printf("s: %s\n", s);

free(s);
```

**Still problematic**: What if user types 101+ characters?

**Better**: Use `get_string()` which handles this safely!

---

## 📁 File I/O (Input/Output)

### Opening Files

```c
FILE *file = fopen("filename.txt", "mode");
```

**Common modes**:

- `"r"` - read
- `"w"` - write (overwrites)
- `"a"` - append
- `"rb"` - read binary
- `"wb"` - write binary

### Writing to a File

```c
#include <stdio.h>

int main(void)
{
    // Open file for appending
    FILE *file = fopen("phonebook.csv", "a");
    if (file == NULL)
    {
        return 1;  // Error handling
    }

    // Get data
    char *name = get_string("Name: ");
    char *number = get_string("Number: ");

    // Write to file (like printf, but to file)
    fprintf(file, "%s,%s\n", name, number);

    // IMPORTANT: Close the file!
    fclose(file);

    return 0;
}
```

**Key functions**:

- `fopen()` - open file
- `fprintf()` - write to file (like `printf`)
- `fclose()` - close file (flushes buffers)

### Reading from a File

```c
FILE *file = fopen("data.txt", "r");
if (file == NULL)
{
    return 1;
}

char buffer[100];
while (fgets(buffer, 100, file) != NULL)
{
    printf("%s", buffer);  // Print each line
}

fclose(file);
```

### Copying a File

```c
typedef unsigned char BYTE;

int main(int argc, char *argv[])
{
    FILE *src = fopen(argv[1], "rb");   // Source (read binary)
    FILE *dst = fopen(argv[2], "wb");   // Destination (write binary)

    BYTE b;
    while (fread(&b, sizeof(b), 1, src) != 0)  // Read one byte
    {
        fwrite(&b, sizeof(b), 1, dst);          // Write one byte
    }

    fclose(dst);
    fclose(src);
}
```

**Usage**: `./cp source.txt destination.txt`

---

## 📊 Memory Layout

### The Four Regions

```
┌─────────────────┐
│  Machine Code   │ ← Your compiled program
├─────────────────┤
│  Globals        │ ← Global variables
├─────────────────┤
│  Heap ↓         │ ← malloc() allocates here (grows down)
│                 │
│                 │
│                 │
│  Stack ↑        │ ← Function calls, local variables (grows up)
└─────────────────┘
```

**Stack**:

- Function calls
- Local variables
- Automatic (freed when function returns)

**Heap**:

- `malloc()` allocations
- Manual (`free()` required)
- Can grow large

**If they meet**: Stack/heap overflow!

---

## 💡 Key Concepts Summary

### Pointers

| Symbol            | Meaning              | Example   |
| ----------------- | -------------------- | --------- |
| `*` (type)        | Declare pointer      | `int *p;` |
| `&`               | Get address          | `&x`      |
| `*` (dereference) | Get value at address | `*p`      |

### Memory Functions

| Function       | Purpose               | Example                 |
| -------------- | --------------------- | ----------------------- |
| `malloc(n)`    | Allocate n bytes      | `char *s = malloc(50);` |
| `free(p)`      | Free allocated memory | `free(s);`              |
| `sizeof(type)` | Get size of type      | `sizeof(int)`           |

### String Functions

| Function            | Purpose           | Example                  |
| ------------------- | ----------------- | ------------------------ |
| `strlen(s)`         | Get string length | `int len = strlen(s);`   |
| `strcmp(s, t)`      | Compare strings   | `if (strcmp(s, t) == 0)` |
| `strcpy(dest, src)` | Copy string       | `strcpy(t, s);`          |

### File Functions

| Function             | Purpose        | Example                          |
| -------------------- | -------------- | -------------------------------- |
| `fopen(name, mode)`  | Open file      | `FILE *f = fopen("x.txt", "r");` |
| `fclose(file)`       | Close file     | `fclose(f);`                     |
| `fprintf(file, ...)` | Write to file  | `fprintf(f, "%s", s);`           |
| `fread(...)`         | Read from file | `fread(&b, 1, 1, f);`            |

---

## ⚠️ Common Mistakes

### 1. Forgetting `&` with `scanf()`

```c
// WRONG
int n;
scanf("%i", n);

// CORRECT
int n;
scanf("%i", &n);
```

### 2. Not Freeing Memory

```c
// WRONG - memory leak
char *s = malloc(50);
// ... use s ...
// forgot free(s)!

// CORRECT
char *s = malloc(50);
// ... use s ...
free(s);
```

### 3. Using `==` for Strings

```c
// WRONG
if (s == t)

// CORRECT
if (strcmp(s, t) == 0)
```

### 4. Forgetting `+ 1` for Null Terminator

```c
// WRONG
char *t = malloc(strlen(s));  // Too small!

// CORRECT
char *t = malloc(strlen(s) + 1);  // Room for '\0'
```

### 5. Not Checking `NULL`

```c
// WRONG
char *s = malloc(100);
strcpy(s, "Hi");  // Crashes if malloc failed!

// CORRECT
char *s = malloc(100);
if (s == NULL) return 1;
strcpy(s, "Hi");
```

---

## ✅ Self-Check Questions

1. Can I explain what a pointer is and why we use them?
2. Do I understand the difference between `&` and `*`?
3. Can I explain why `string` is just `char *`?
4. Do I know when to use `malloc()` and `free()`?
5. Can I explain why `==` doesn't work for strings?
6. Do I understand pass by value vs pass by reference?
7. Can I identify potential buffer overflows?
8. Do I know how to check for memory leaks with Valgrind?

---

## 🚀 Practice Exercises

### Beginner

- [ ] Print the address of a variable
- [ ] Create a pointer to an integer
- [ ] Print a string character by character using pointers
- [ ] Use `malloc()` and `free()` for a simple array
- [ ] Compare two strings correctly

### Intermediate

- [ ] Copy a string manually (character by character)
- [ ] Implement a swap function using pointers
- [ ] Write to a file and read it back
- [ ] Find and fix memory leaks with Valgrind
- [ ] Use pointer arithmetic to navigate an array

### Advanced

- [ ] Implement your own `strlen()` using pointers
- [ ] Create a program that manipulates image files
- [ ] Handle multiple levels of pointers (`int **p`)
- [ ] Implement dynamic array resizing
- [ ] Write a file copying program

---

## 🎓 Study Tips

### Understanding Pointers

1. **Draw diagrams** - Visual representations help immensely
2. **Use concrete addresses** - `0x123` instead of just "address"
3. **Practice with small examples** - Don't jump to complex code
4. **Trace execution step-by-step** - Write down values and addresses

### Mastering Memory Management

1. **Every `malloc()` needs a `free()`** - Make this automatic
2. **Check for `NULL`** - Always, no exceptions
3. **Use Valgrind religiously** - Catch errors early
4. **Initialize your memory** - Don't trust garbage values

### Debugging Memory Issues

1. **Print addresses** with `%p` - See what's pointing where
2. **Use Valgrind** - Your best friend for memory debugging
3. **Check array bounds** - Common source of errors
4. **Verify string lengths** - Don't forget `+ 1` for `\0`

---

## 📌 Important Reminders

1. **Pointers store addresses, not values** (unless you dereference)
2. **`string` is not a primitive type** - it's `char *`
3. **Always free what you malloc** - prevent memory leaks
4. **Array names are pointers** - `arr` vs `&arr[0]` is the same
5. **Pass addresses to modify values** in functions (`&x`)
6. **Check for NULL** after `malloc()` and `fopen()`
7. **Close files** with `fclose()` when done
8. **Hexadecimal starts with `0x`** - convention, not requirement
9. **Buffer overflows are security risks** - allocate enough!
10. **When in doubt, use Valgrind** - it catches what you miss

---

_Memory management is challenging but fundamental. Take your time, draw diagrams, and practice with simple examples before moving to complex programs!_
