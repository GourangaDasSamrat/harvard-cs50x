# CS50 Lecture 2 - Arrays & Debugging

## Personal Learning Notes

---

## 🎯 Key Takeaways

- **Debugging is essential** - Everyone makes mistakes; learning to fix them is critical
- **Arrays store data back-to-back** - Sequential memory locations for related data
- **Strings are character arrays** - Ending with `\0` (NUL terminator)
- **Compilation has 4 steps** - Preprocessing, compiling, assembling, linking
- **Command-line arguments** - Programs can accept input at runtime
- **Data types have sizes** - Memory allocation matters

---

## 🐛 Debugging Techniques

### Why Debugging Matters

**Every programmer makes mistakes!** Debugging is the process of finding and fixing bugs (errors) in your code.

### Method 1: Printf Debugging

**Most common and simple** - Print values to see what's happening

**Example problem**:

```c
// Buggy - prints 4 blocks instead of 3
#include <stdio.h>

int main(void)
{
    for (int i = 0; i <= 3; i++)  // Bug: <= should be <
    {
        printf("#\n");
    }
}
```

**Debug by printing the counter**:

```c
#include <stdio.h>

int main(void)
{
    for (int i = 0; i <= 3; i++)
    {
        printf("i is %i\n", i);  // Debug line
        printf("#\n");
    }
}
```

**Output**:

```
i is 0
#
i is 1
#
i is 2
#
i is 3
#
```

**Aha!** Loop runs 4 times (0, 1, 2, 3) instead of 3. Fix: Change `<=` to `<`

**Fixed code**:

```c
for (int i = 0; i < 3; i++)
{
    printf("#\n");
}
```

### Method 2: Using debug50

**debug50** is VS Code's debugger - lets you step through code line by line

**How to use**:

1. **Set a breakpoint** - Click left of line number (red dot appears)
2. **Run debugger** - Type `debug50 ./program` in terminal
3. **Step through code** - Use controls at top:
   - **Step Over** - Execute current line, move to next
   - **Step Into** - Go inside function calls
   - **Continue** - Run until next breakpoint

**What you see**:

- Code pauses at breakpoint (line highlighted in gold)
- **Local variables** shown in left panel with current values
- Can watch values change as you step through

**When to use**: When printf isn't enough, or you need to see exact execution flow

### Method 3: Rubber Duck Debugging

**Talk through your problem** - Explain code to an inanimate object (or person)

**How it works**:

1. Get a rubber duck (or any object)
2. Explain your code line by line to the duck
3. Describe what each line _should_ do
4. Often you'll spot the error while explaining!

**Why it works**: Forces you to think through the logic step-by-step

**Alternative**: Use **CS50 Duck** at cs50.ai - an AI assistant for debugging

### Method 4: Common Bug Checklist

Before asking for help, check:

- [ ] Did I compile? (`make program`)
- [ ] Any typos in variable names?
- [ ] Correct comparison operators? (`<` vs `<=`, `==` vs `=`)
- [ ] Off-by-one errors in loops?
- [ ] Missing semicolons?
- [ ] Missing `#include` statements?
- [ ] Correct format codes in printf? (`%i`, `%s`, `%f`)

---

## 🔧 Common Compilation Errors

### Error 1: Missing Header File

```c
// WRONG - missing #include
int main(void)
{
    printf("hello, world\n");  // Error: printf not recognized!
}
```

**Error message**: `implicit declaration of function 'printf'`

**Fix**:

```c
#include <stdio.h>  // Add this!

int main(void)
{
    printf("hello, world\n");
}
```

### Error 2: Misspelled Header

```c
// WRONG - stdio.h misspelled as studio.h
#include <studio.h>

int main(void)
{
    printf("hello, world\n");
}
```

**Error message**: `studio.h: No such file or directory`

**Fix**: Correct spelling to `<stdio.h>`

### Error 3: Multiple Errors

```c
// WRONG - many errors!
#include <stdio.h>

int main(void)
{
    name = get_string("What's your name? ")  // Missing type, semicolon
    printf("hello, world\n");  // Not using name variable
}
```

**Errors**:

1. Missing `#include <cs50.h>` for `get_string()`
2. Missing type for `name` (should be `string name`)
3. Missing semicolon after `get_string()`
4. `printf` doesn't use `name`

**Fixed**:

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name? ");
    printf("hello, %s\n", name);
}
```

---

## ⚙️ How Compilation Works

### The Four Steps

**When you type `make program`**, here's what happens:

```
Source Code → Preprocessing → Compiling → Assembling → Linking → Executable
```

### Step 1: Preprocessing

**What it does**: Copy/paste header files into your code

**Before**:

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string name = get_string("What's your name? ");
    printf("hello, %s\n", name);
}
```

**After preprocessing**:

```c
// All function declarations from cs50.h and stdio.h are inserted
string get_string(string prompt);
int printf(string format, ...);

int main(void)
{
    string name = get_string("What's your name? ");
    printf("hello, %s\n", name);
}
```

### Step 2: Compiling

**What it does**: Convert C code to **assembly language**

**Assembly code** (low-level, CPU-specific):

```assembly
main:
    pushq %rbp
    movq %rsp, %rbp
    subq $16, %rsp
    movabsq $.L.str, %rsi
    callq get_string
    ...
```

**Don't worry about understanding this!** Just know it's an intermediate step.

### Step 3: Assembling

**What it does**: Convert assembly to **machine code** (binary)

**Machine code**:

```
01111111010001010100110001000110
00000010000000010000000100000000
00000000000000000000000000000000
...
```

**This is what the CPU actually executes!**

### Step 4: Linking

**What it does**: Combine your code with library code

- Your compiled code
- - Pre-compiled library code (stdio, cs50, etc.)
- = Final executable program

**Result**: A file you can run (e.g., `./hello`)

### Using clang Directly

**Normally**: `make hello` (easy!)

**Under the hood**: `clang -o hello hello.c -lcs50`

**Breaking it down**:

- `clang` - the compiler
- `-o hello` - output file named "hello"
- `hello.c` - source file
- `-lcs50` - link CS50 library

**Stick with `make`** - it's simpler and handles everything!

---

## 📦 Data Types & Memory

### Memory Requirements

Each data type requires specific memory:

| Type     | Size    | Example Values            |
| -------- | ------- | ------------------------- |
| `bool`   | 1 byte  | true, false               |
| `char`   | 1 byte  | 'a', 'Z', '5'             |
| `int`    | 4 bytes | -2147483648 to 2147483647 |
| `long`   | 8 bytes | Very large integers       |
| `float`  | 4 bytes | 3.14, 2.5                 |
| `double` | 8 bytes | More precise decimals     |
| `string` | ? bytes | Depends on length         |

### Visualizing Memory

**A single `char`** (1 byte):

```
┌───┐
│ H │
└───┘
```

**An `int`** (4 bytes):

```
┌───┬───┬───┬───┐
│   │   │   │   │
└───┴───┴───┴───┘
```

**Multiple variables**:

```
Memory:
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ 72│   │   │   │ 73│   │   │   │ 33│   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
  score1 (int)    score2 (int)    score3 (int)
```

---

## 📊 Arrays - Data in Sequence

### What is an Array?

**Array**: Collection of values of the **same type**, stored **back-to-back** in memory

**Think of it as**: Numbered boxes in a row

### Creating Arrays

**Basic syntax**:

```c
type name[size];
```

**Example**:

```c
int scores[3];  // Array of 3 integers
```

**Memory visualization**:

```
┌────┬────┬────┐
│ ?? │ ?? │ ?? │  (uninitialized)
└────┴────┴────┘
 [0]  [1]  [2]
```

### Accessing Array Elements

**Index**: Position in array (starts at 0!)

```c
scores[0] = 72;  // First element
scores[1] = 73;  // Second element
scores[2] = 33;  // Third element
```

**After assignment**:

```
┌────┬────┬────┐
│ 72 │ 73 │ 33 │
└────┴────┴────┘
 [0]  [1]  [2]
```

**IMPORTANT**: Arrays are **zero-indexed**!

- First element: `[0]`
- Second element: `[1]`
- Third element: `[2]`

### Array Example: Calculating Average

**Version 1: Without arrays** (inefficient):

```c
#include <stdio.h>

int main(void)
{
    int score1 = 72;
    int score2 = 73;
    int score3 = 33;

    printf("Average: %f\n", (score1 + score2 + score3) / 3.0);
}
```

**Version 2: With arrays**:

```c
#include <stdio.h>

int main(void)
{
    int scores[3];
    scores[0] = 72;
    scores[1] = 73;
    scores[2] = 33;

    printf("Average: %f\n", (scores[0] + scores[1] + scores[2]) / 3.0);
}
```

**Version 3: With loop** (best!):

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    int scores[3];

    // Get scores from user
    for (int i = 0; i < 3; i++)
    {
        scores[i] = get_int("Score: ");
    }

    // Calculate average
    printf("Average: %f\n", (scores[0] + scores[1] + scores[2]) / 3.0);
}
```

### Using Constants for Array Size

**Problem**: Hard-coded `3` everywhere - what if we want 4 scores?

**Solution**: Use a constant!

```c
#include <cs50.h>
#include <stdio.h>

const int N = 3;  // Constant - never changes

int main(void)
{
    int scores[N];  // Use N instead of 3

    for (int i = 0; i < N; i++)
    {
        scores[i] = get_int("Score: ");
    }

    printf("Average: %f\n", (scores[0] + scores[1] + scores[2]) / 3.0);
}
```

**Benefits**:

- Change `N` once, updates everywhere
- Code is more maintainable
- Convention: Constants in UPPERCASE

### Passing Arrays to Functions

**Arrays can be function parameters!**

```c
#include <cs50.h>
#include <stdio.h>

const int N = 3;

float average(int length, int array[]);

int main(void)
{
    int scores[N];

    for (int i = 0; i < N; i++)
    {
        scores[i] = get_int("Score: ");
    }

    printf("Average: %f\n", average(N, scores));
}

float average(int length, int array[])
{
    int sum = 0;
    for (int i = 0; i < length; i++)
    {
        sum += array[i];
    }
    return sum / (float) length;  // Cast to float for decimal result
}
```

**Key points**:

- Function parameter: `int array[]`
- Pass array name: `average(N, scores)`
- Also pass length so function knows array size

---

## 🔤 Strings - Arrays of Characters

### The Truth About Strings

**A string is just an array of `char`** with a special ending!

```c
string s = "HI!";
```

**In memory**:

```
┌───┬───┬───┬───┐
│ H │ I │ ! │ \0│
└───┴───┴───┴───┘
 [0] [1] [2] [3]
```

**`\0`** = NUL character (null terminator) - marks the end of string

### String as Character Array

**Printing individual characters**:

```c
#include <stdio.h>

int main(void)
{
    char c1 = 'H';
    char c2 = 'I';
    char c3 = '!';

    printf("%c%c%c\n", c1, c2, c3);  // Prints: HI!
}
```

**Accessing string characters**:

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string s = "HI!";

    printf("%c%c%c\n", s[0], s[1], s[2]);  // Prints: HI!
}
```

**It's an array!** Use bracket notation to access each character.

### ASCII Values in Strings

**Printing ASCII codes**:

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string s = "HI!";

    printf("%i %i %i %i\n", s[0], s[1], s[2], s[3]);
    // Prints: 72 73 33 0
    //         H  I  !  \0
}
```

**Recall ASCII table**:

- `H` = 72
- `I` = 73
- `!` = 33
- `\0` = 0

### The NUL Terminator

**Why `\0` is important**:

- Marks end of string
- Functions like `printf` know where to stop
- Without it, undefined behavior!

**Example**:

```c
string s = "HI!";

// In memory:
// H    I    !    \0
// 72   73   33   0
// [0]  [1]  [2]  [3]
```

**Note**: NUL (one L) = `\0` character ≠ NULL (two Ls) = null pointer

### Array of Strings

**Strings are arrays of chars, so array of strings = 2D array!**

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string words[2];
    words[0] = "HI!";
    words[1] = "BYE!";

    printf("%s\n", words[0]);  // Prints: HI!
    printf("%s\n", words[1]);  // Prints: BYE!
}
```

**Accessing individual characters**:

```c
printf("%c%c%c\n", words[0][0], words[0][1], words[0][2]);  // HI!
printf("%c%c%c%c\n", words[1][0], words[1][1], words[1][2], words[1][3]);  // BYE!
```

**Syntax**: `words[string_index][char_index]`

---

## 📏 String Length

### Finding String Length Manually

**Problem**: How long is a string?

**Solution**: Count until you hit `\0`!

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string name = get_string("Name: ");

    // Count characters until '\0'
    int n = 0;
    while (name[n] != '\0')
    {
        n++;
    }

    printf("%i\n", n);
}
```

**How it works**:

- Start counter at 0
- Check each character
- If not `\0`, increment counter
- When we hit `\0`, we're done!

### Creating a Function

**Better: Abstract into reusable function**

```c
#include <cs50.h>
#include <stdio.h>

int string_length(string s);

int main(void)
{
    string name = get_string("Name: ");
    int length = string_length(name);
    printf("%i\n", length);
}

int string_length(string s)
{
    int n = 0;
    while (s[n] != '\0')
    {
        n++;
    }
    return n;
}
```

### Using strlen() from string.h

**Even better: Use built-in function!**

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>  // Required for strlen()

int main(void)
{
    string name = get_string("Name: ");
    int length = strlen(name);
    printf("%i\n", length);
}
```

**Why reinvent the wheel?** Library functions are tested, optimized, and reliable!

---

## 🔡 String Manipulation

### Converting to Uppercase

**Method 1: Manual ASCII manipulation**

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    string s = get_string("Before: ");
    printf("After:  ");

    for (int i = 0, n = strlen(s); i < n; i++)
    {
        if (s[i] >= 'a' && s[i] <= 'z')  // If lowercase
        {
            printf("%c", s[i] - 32);  // Convert to uppercase
        }
        else
        {
            printf("%c", s[i]);  // Keep as is
        }
    }
    printf("\n");
}
```

**Why `- 32` works**:

- Lowercase 'a' = 97
- Uppercase 'A' = 65
- Difference = 32

**From ASCII table**:

```
A = 65    a = 97    (difference of 32)
B = 66    b = 98
C = 67    c = 99
...
```

### Method 2: Using ctype.h Library

**Much simpler!**

```c
#include <cs50.h>
#include <ctype.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    string s = get_string("Before: ");
    printf("After:  ");

    for (int i = 0, n = strlen(s); i < n; i++)
    {
        printf("%c", toupper(s[i]));  // Convert each character
    }
    printf("\n");
}
```

**Key functions from ctype.h**:

- `toupper(c)` - Convert to uppercase (does nothing if already uppercase)
- `tolower(c)` - Convert to lowercase
- `islower(c)` - Check if lowercase
- `isupper(c)` - Check if uppercase
- `isalpha(c)` - Check if letter
- `isdigit(c)` - Check if digit

**Benefit**: `toupper()` only affects lowercase letters, leaves others unchanged!

---

## 💻 Command-Line Arguments

### What Are Command-Line Arguments?

**Arguments passed when running a program**:

```bash
./greet David
```

Here, `David` is a command-line argument.

### Basic Example

**Without command-line arguments**:

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    string answer = get_string("What's your name? ");
    printf("hello, %s\n", answer);
}
```

**With command-line arguments**:

```c
#include <stdio.h>

int main(int argc, string argv[])
{
    if (argc == 2)
    {
        printf("hello, %s\n", argv[1]);
    }
    else
    {
        printf("hello, world\n");
    }
}
```

**Usage**:

```bash
./greet David    # Prints: hello, David
./greet          # Prints: hello, world
```

### Understanding argc and argv

**`argc`** = Argument Count

- How many arguments were provided
- Includes program name itself

**`argv`** = Argument Vector (array)

- Array of strings
- `argv[0]` = program name
- `argv[1]` = first actual argument
- `argv[2]` = second argument
- etc.

**Example**:

```bash
./greet David Malan
```

**Results in**:

- `argc = 3`
- `argv[0] = "./greet"`
- `argv[1] = "David"`
- `argv[2] = "Malan"`

### Printing All Arguments

```c
#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[])
{
    for (int i = 0; i < argc; i++)
    {
        printf("%s\n", argv[i]);
    }
}
```

**Running `./greet David Malan`**:

```
./greet
David
Malan
```

### Checking Argument Count

**Good practice**: Validate number of arguments

```c
#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[])
{
    if (argc != 2)  // Expecting exactly 1 argument (plus program name)
    {
        printf("Usage: ./greet name\n");
        return 1;  // Exit with error
    }

    printf("hello, %s\n", argv[1]);
    return 0;  // Exit successfully
}
```

---

## 🚪 Exit Status

### What is Exit Status?

**Every program returns a code** when it finishes:

- `0` = Success (everything went well)
- `1` (or other non-zero) = Error/Failure

### Returning Exit Status

```c
#include <cs50.h>
#include <stdio.h>

int main(int argc, string argv[])
{
    if (argc != 2)
    {
        printf("Missing command-line argument\n");
        return 1;  // Error!
    }

    printf("hello, %s\n", argv[1]);
    return 0;  // Success!
}
```

### Checking Exit Status

**In terminal**:

```bash
./status David
hello, David

echo $?
0           # Success!
```

```bash
./status
Missing command-line argument

echo $?
1           # Error!
```

**`echo $?`** shows exit status of last command

### Why It Matters

**Scripts can check if programs succeeded**:

```bash
./program
if [ $? -eq 0 ]; then
    echo "Success!"
else
    echo "Failed!"
fi
```

---

## 💡 Key Concepts Summary

### Arrays

| Concept          | Syntax             | Example           |
| ---------------- | ------------------ | ----------------- |
| Declaration      | `type name[size];` | `int scores[5];`  |
| Access           | `name[index]`      | `scores[0] = 95;` |
| Pass to function | `function(array)`  | `average(scores)` |

### Strings

| Concept     | Detail                           |
| ----------- | -------------------------------- |
| Definition  | Array of `char` ending with `\0` |
| Access char | `s[index]`                       |
| Get length  | `strlen(s)` from `string.h`      |
| Compare     | Use `strcmp()`, not `==`         |
| Manipulate  | Use `ctype.h` functions          |

### Command-Line Arguments

| Variable  | Meaning        | Example     |
| --------- | -------------- | ----------- |
| `argc`    | Argument count | `3`         |
| `argv[0]` | Program name   | `"./greet"` |
| `argv[1]` | First argument | `"David"`   |

---

## ⚠️ Common Mistakes

### 1. Array Index Out of Bounds

```c
// WRONG
int scores[3];
scores[3] = 100;  // ERROR! Index 3 doesn't exist (0, 1, 2)

// CORRECT
int scores[3];
scores[2] = 100;  // Last valid index
```

### 2. Off-by-One in Loops

```c
// WRONG
for (int i = 0; i <= 3; i++)  // Runs 4 times (0,1,2,3)

// CORRECT
for (int i = 0; i < 3; i++)   // Runs 3 times (0,1,2)
```

### 3. Forgetting NUL Terminator

**Strings need `\0` at the end!** Functions like `printf` depend on it.

### 4. Not Checking argc

```c
// WRONG - might crash!
printf("%s\n", argv[1]);  // What if no arguments?

// CORRECT
if (argc >= 2)
{
    printf("%s\n", argv[1]);
}
```

### 5. Integer Division in Average

```c
// WRONG
int avg = (a + b + c) / 3;  // Truncates decimal!

// CORRECT
float avg = (a + b + c) / 3.0;  // Use 3.0 for float division
```

---

## ✅ Self-Check Questions

1. Can I explain what an array is and how to create one?
2. Do I understand zero-indexing? (First element is `[0]`)
3. Can I explain why strings end with `\0`?
4. Do I know how to find string length with `strlen()`?
5. Can I use command-line arguments (`argc`, `argv`)?
6. Do I understand the difference between `return 0` and `return 1`?
7. Can I debug code using printf statements?
8. Do I know when to use `toupper()` vs manual ASCII math?

---

## 🚀 Practice Exercises

### Beginner

- [ ] Create array of 5 integers, print each
- [ ] Calculate average of user-input numbers
- [ ] Print each character of a string
- [ ] Convert string to uppercase
- [ ] Use debug50 on a simple program

### Intermediate

- [ ] Write function to reverse a string
- [ ] Accept and use command-line arguments
- [ ] Create array of strings, print each
- [ ] Implement your own strlen() function
- [ ] Calculate reading level of text

### Advanced

- [ ] Create 2D array (array of arrays)
- [ ] Sort an array of integers
- [ ] Parse complex command-line arguments
- [ ] Implement Caesar cipher
- [ ] Build a gradebook program

---

## 🎓 Study Tips

### Mastering Arrays

1. **Draw memory diagrams** - Visualize the boxes
2. **Always start at 0** - Remember zero-indexing
3. **Check bounds** - Don't access outside array
4. **Use constants** - `const int N = 5;` instead of magic numbers

### Understanding Strings

1. **Think of them as arrays** - Because they are!
2. **Remember the `\0`** - Every string ends with it
3. **Use library functions** - Don't reinvent `strlen()`, `toupper()`, etc.
4. **Never compare with `==`** - Use `strcmp()` (covered next week)

### Debugging Effectively

1. **Printf is your friend** - Print values liberally
2. **Use debug50** - Step through confusing code
3. **Rubber duck** - Talk through logic
4. **Check error messages** - Read them carefully!

---

## 📌 Important Reminders

1. **Arrays are zero-indexed** - First element is `[0]`, not `[1]`
2. **Strings end with `\0`** - The NUL terminator
3. **`argv[0]` is program name** - Actual arguments start at `argv[1]`
4. **Return 0 for success** - Non-zero for errors
5. **Use constants** - `const int N` instead of hardcoded numbers
6. **Check argc** - Before accessing `argv` elements
7. **Library functions exist** - Use `strlen()`, `toupper()`, etc.
8. **Compilation has steps** - Preprocessing, compiling, assembling, linking
9. **Debug early** - Don't wait until code is completely broken
10. **Every program has bugs** - Even experts make mistakes!

---

_Arrays and strings are fundamental to C programming. Take time to really understand how they work in memory - it will pay off throughout the course!_
