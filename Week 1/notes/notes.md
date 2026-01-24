# CS50 Lecture 1 - Introduction to C Programming

## Personal Learning Notes

---

## 🎯 Key Takeaways

- **C is a compiled language**: Source code (human-readable) → Compiler → Machine code (binary)
- **Syntax matters**: Every character counts - semicolons, quotes, brackets all have specific purposes
- **Three axes of code quality**: Correctness, Design, and Style
- **Building blocks transfer**: Functions, conditionals, loops, and variables from Scratch work the same way in C

---

## 🛠️ Development Environment

### VS Code for CS50 (cs50.dev)

**Why use cs50.dev?**

- All necessary software pre-installed
- No manual setup headaches
- Designed specifically for this course

**IDE Layout**:

- **Left**: File explorer
- **Middle**: Text editor (where you write code)
- **Bottom**: Terminal/CLI (Command Line Interface)

### Essential Terminal Commands

```bash
code filename.c    # Create/open a file in the text editor
make filename      # Compile your C program
./filename         # Run your compiled program
ls                 # List files in current directory
cd                 # Change directory
mkdir              # Make directory
rm                 # Remove file
cp                 # Copy file
mv                 # Move/rename file
```

---

## 📝 Your First C Program

### The Basic Workflow

**Three commands to remember**:

```bash
code hello.c    # 1. Create and write code
make hello      # 2. Compile the code
./hello         # 3. Run the program
```

### Hello World Program

```c
// A program that says hello to the world
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
}
```

**Breaking it down**:

- `//` = Comment (for humans, compiler ignores)
- `#include <stdio.h>` = Import standard input/output library
- `int main(void)` = Main function where program starts
- `printf()` = Function to print text
- `"hello, world\n"` = String to print
- `\n` = Newline character (escape sequence)
- `;` = End of statement (REQUIRED!)

---

## 🔤 Important Concepts

### 1. **Compilation Process**

**Source Code** → **Compiler** → **Machine Code**

- **Source code**: Human-readable (hello.c)
- **Machine code**: Binary that computer executes (hello)
- **make**: Build tool that runs the compiler for you

### 2. **Libraries and Header Files**

**stdio.h** (Standard Input/Output):

- Provides `printf()`, `scanf()`, and other I/O functions
- Syntax: `#include <stdio.h>`

**cs50.h** (CS50 Library):

- Training wheels for beginners
- Provides: `get_int()`, `get_string()`, `get_float()`, `get_char()`, etc.
- Syntax: `#include <cs50.h>`

**Manual Pages**: Check docs at manual.cs50.io for function details

### 3. **Escape Characters**

```c
\n    // Newline (line break)
\r    // Carriage return (start of line)
\"    // Double quote
\'    // Single quote
\\    // Backslash
```

---

## 📊 Data Types

### Common Types in C

```c
int        // Integer (whole numbers): -2, -1, 0, 1, 2
float      // Decimal numbers: 3.14, 2.5
double     // More precise decimal numbers
char       // Single character: 'a', 'Z', '5'
string     // Series of characters: "hello"
bool       // True or false (from cs50.h)
long       // Larger integer
```

### Type Limits & Overflow

**int limits**:

- Signed: -2,147,483,648 to 2,147,483,647
- Unsigned: 0 to 4,294,967,295

**Integer overflow**: When you exceed the max value, wraps around unpredictably!

```c
int dollars = 2147483647;
dollars++;  // Overflow! Becomes negative or wraps
```

**Solution**: Use `long` for larger numbers

```c
long dollars = 1;  // Can store much larger values
printf("%li\n", dollars);  // Use %li for long
```

---

## 🎨 Format Codes

**Format codes** tell `printf()` what type of data to expect:

```c
%c     // char
%f     // float
%i     // int
%li    // long
%s     // string
%.2f   // float with 2 decimal places
%.50f  // float with 50 decimal places
```

**Example**:

```c
int age = 25;
printf("I am %i years old\n", age);  // Output: I am 25 years old

string name = "Alice";
printf("Hello, %s\n", name);  // Output: Hello, Alice
```

---

## 🔢 Variables

### Declaration and Assignment

```c
// Declare and initialize
int counter = 0;

// Declare first, assign later
int x;
x = 5;

// Get user input
int age = get_int("What's your age? ");
string name = get_string("What's your name? ");
```

### Variable Operations

```c
// Incrementing
counter = counter + 1;  // Add 1
counter += 1;           // Shorthand
counter++;              // Even shorter!

// Decrementing
counter--;              // Subtract 1

// Other operators
x *= 2;   // Multiply by 2
x /= 2;   // Divide by 2
x += 5;   // Add 5
```

---

## 🔀 Conditionals

### Basic If Statement

```c
if (x < y)
{
    printf("x is less than y\n");
}
```

### If-Else

```c
if (x < y)
{
    printf("x is less than y\n");
}
else
{
    printf("x is not less than y\n");
}
```

### If-Else If-Else

```c
if (x < y)
{
    printf("x is less than y\n");
}
else if (x > y)
{
    printf("x is greater than y\n");
}
else
{
    printf("x is equal to y\n");
}
```

### Comparison Operators

```c
x < y      // Less than
x > y      // Greater than
x <= y     // Less than or equal
x >= y     // Greater than or equal
x == y     // Equal to (NOTE: Double equals!)
x != y     // Not equal to
```

### Logical Operators

```c
// OR (||): True if at least one condition is true
if (c == 'y' || c == 'Y')
{
    printf("Yes\n");
}

// AND (&&): True only if both conditions are true
if (x > 0 && x < 100)
{
    printf("x is between 0 and 100\n");
}

// NOT (!): Reverses the condition
if (!(x == 0))
{
    printf("x is not zero\n");
}
```

---

## 🔁 Loops

### While Loop

```c
// Basic while loop
int i = 0;
while (i < 3)
{
    printf("meow\n");
    i++;
}
```

**Pattern**: Initialize → Check condition → Execute → Update

### For Loop (Preferred for counting)

```c
// For loop structure
for (int i = 0; i < 3; i++)
{
    printf("meow\n");
}
```

**Three parts**:

1. `int i = 0` - Initialize counter
2. `i < 3` - Condition to check
3. `i++` - Update counter after each iteration

### Do-While Loop

```c
// Runs at least once, then checks condition
int n;
do
{
    n = get_int("What's n? ");
}
while (n < 0);
```

**Use when**: You want the code to run at least once before checking condition

### Loop Control

```c
// Infinite loop
while (true)
{
    printf("Forever!\n");
}
// Stop with Ctrl+C

// Continue: Skip rest of current iteration
if (n < 0)
{
    continue;  // Jump to next iteration
}

// Break: Exit loop entirely
if (n >= 0)
{
    break;  // Exit the loop
}
```

---

## 🎯 Functions

### Why Functions?

- **Abstraction**: Simplify complex problems
- **Reusability**: Write once, use many times
- **Organization**: Make code cleaner and easier to read

### Function Structure

```c
// Function prototype (declaration)
void meow(void);

int main(void)
{
    meow();  // Function call
}

// Function definition
void meow(void)
{
    printf("meow\n");
}
```

**Parts**:

- **Return type**: `void` (returns nothing) or `int`, `string`, etc.
- **Function name**: `meow`
- **Parameters**: `void` (no parameters) or variable list
- **Prototype**: Tells compiler the function exists (must come before use)

### Functions with Parameters

```c
// Function that takes input
void meow(int n);

int main(void)
{
    meow(3);  // Meow 3 times
}

void meow(int n)
{
    for (int i = 0; i < n; i++)
    {
        printf("meow\n");
    }
}
```

### Functions with Return Values

```c
// Function that returns a value
int get_positive_int(void);

int main(void)
{
    int number = get_positive_int();
    printf("You entered: %i\n", number);
}

int get_positive_int(void)
{
    int n;
    do
    {
        n = get_int("Enter positive number: ");
    }
    while (n < 1);
    return n;  // Send value back to caller
}
```

### Variable Scope

```c
int main(void)
{
    int n = 3;  // n exists only in main()
    meow(n);    // Copy of n is sent to meow()
}

void meow(int n)
{
    // This n is a COPY, not the same variable
    // Changes here don't affect main's n
    for (int i = 0; i < n; i++)
    {
        printf("meow\n");
    }
}
```

**Key point**: Variables have limited scope - they only exist in the function where they're declared!

---

## 🎨 Nested Loops (Mario Example)

### Single Row

```c
// Print 4 question marks horizontally
for (int i = 0; i < 4; i++)
{
    printf("?");
}
printf("\n");
// Output: ????
```

### Single Column

```c
// Print 3 bricks vertically
for (int i = 0; i < 3; i++)
{
    printf("#\n");
}
// Output:
// #
// #
// #
```

### Grid (Nested Loop)

```c
// Print 3x3 grid
for (int i = 0; i < 3; i++)      // Rows
{
    for (int j = 0; j < 3; j++)  // Columns
    {
        printf("#");
    }
    printf("\n");  // New line after each row
}
// Output:
// ###
// ###
// ###
```

**Pattern**: Outer loop = rows, Inner loop = columns

### Using Constants

```c
const int n = 3;  // Cannot be changed

for (int i = 0; i < n; i++)
{
    for (int j = 0; j < n; j++)
    {
        printf("#");
    }
    printf("\n");
}
```

### Abstraction with Helper Function

```c
void print_row(int width);

int main(void)
{
    const int n = 3;
    for (int i = 0; i < n; i++)
    {
        print_row(n);
    }
}

void print_row(int width)
{
    for (int i = 0; i < width; i++)
    {
        printf("#");
    }
    printf("\n");
}
```

---

## ➕ Operators

### Arithmetic Operators

```c
+     // Addition
-     // Subtraction
*     // Multiplication
/     // Division
%     // Modulo (remainder)

// Examples
int sum = 5 + 3;        // 8
int diff = 10 - 4;      // 6
int product = 4 * 5;    // 20
int quotient = 10 / 3;  // 3 (integer division!)
int remainder = 10 % 3; // 1
```

---

## ⚠️ Common Pitfalls

### 1. Integer Division (Truncation)

```c
int x = 7;
int y = 2;
printf("%i\n", x / y);  // Output: 3 (not 3.5!)
```

**Why?** Dividing two integers = integer result (decimal part discarded)

**Solutions**:

```c
// Option 1: Cast to float
printf("%f\n", (float) x / y);  // 3.500000

// Option 2: Use floats from the start
float x = 7.0;
float y = 2.0;
printf("%f\n", x / y);  // 3.500000
```

### 2. Floating Point Imprecision

```c
float x = get_float("x: ");  // User enters 1
float y = get_float("y: ");  // User enters 10

printf("%.50f\n", x / y);
// Output: 0.10000000149011611938476562500000000000000000000000
```

**Why?** Computers represent decimals in binary, which can't perfectly represent all decimal numbers.

### 3. Single vs Double Equals

```c
// WRONG - Assignment, not comparison!
if (x = 5)
{
    // Always runs because x = 5 is truthy
}

// CORRECT - Comparison
if (x == 5)
{
    // Runs only if x equals 5
}
```

### 4. Char vs String

```c
char c = 'y';          // Single quotes for char
string s = "yes";      // Double quotes for string

// Comparing chars
if (c == 'y')          // Correct
if (c == "y")          // WRONG! Can't compare char to string
```

---

## ✅ Code Quality: Three Axes

### 1. Correctness

**Question**: Does the code work as intended?

**Check with**:

```bash
check50 <problem-set-identifier>
```

### 2. Design

**Question**: How well is the code designed?

**Evaluate with**:

```bash
design50 filename.c
```

**Good design**:

- No unnecessary repetition
- Uses abstraction appropriately
- Logical organization
- Efficient algorithms

### 3. Style

**Question**: Is the code aesthetically pleasing and consistent?

**Check with**:

```bash
style50 filename.c
```

**Good style**:

- Proper indentation
- Meaningful variable names
- Comments where needed
- Consistent spacing

---

## 💡 Problem-Solving Approach

### Step-by-Step Process

1. **Understand the problem**
   - What are the inputs?
   - What should the output be?
   - What are the constraints?

2. **Write pseudocode**
   - Plan your logic before coding
   - Use plain English

3. **Translate to C**
   - Start simple
   - Test frequently

4. **Debug and refine**
   - Check correctness
   - Improve design
   - Polish style

### Example: Getting Valid Input

**Problem**: Get a positive integer from user

**Pseudocode**:

```
1. Repeat until we get valid input:
   2. Ask user for number
   3. If number is positive, we're done
4. Use the number
```

**C code**:

```c
int n;
do
{
    n = get_int("Positive number: ");
}
while (n < 1);

// Now use n...
```

---

## 📌 Important Terms Glossary

- **Compiler**: Converts source code to machine code
- **Source code**: Human-readable code (.c files)
- **Machine code**: Binary that computer executes
- **CLI**: Command Line Interface (terminal)
- **Header file**: Library declarations (.h files)
- **Prototype**: Function declaration before use
- **Scope**: Where a variable can be accessed
- **Truncation**: Discarding decimal part in integer division
- **Overflow**: Exceeding max value of data type
- **Casting**: Converting one data type to another
- **Escape sequence**: Special characters (e.g., \n)
- **Format code**: Placeholder in printf (%i, %s, etc.)

---

## ✅ Self-Check Questions

1. Can I write a basic C program with proper syntax?
2. Do I understand the difference between `=` and `==`?
3. Can I explain when to use while vs for loops?
4. Do I know how to create and call functions?
5. Can I identify and fix integer division issues?
6. Do I understand variable scope?
7. Can I use nested loops to create patterns?
8. Do I know the difference between char and string?

---

## 🚀 Practice Exercises

### Beginner

- [ ] Print "hello, world" with newline
- [ ] Get user's name and greet them
- [ ] Compare two numbers
- [ ] Print numbers 1-10 using a loop
- [ ] Create a function that prints a pattern

### Intermediate

- [ ] Get valid positive integer from user
- [ ] Print a multiplication table
- [ ] Create a simple calculator
- [ ] Print a pyramid of # symbols
- [ ] Validate user input (y/n)

### Advanced

- [ ] Create Mario-style blocks with variable height
- [ ] Implement multiple helper functions
- [ ] Handle edge cases (overflow, invalid input)
- [ ] Build a complete program with good design

---

## 🔑 Key Differences: Scratch vs C

| Concept       | Scratch          | C                                   |
| ------------- | ---------------- | ----------------------------------- |
| **Variables** | Drag "set" block | `int x = 5;`                        |
| **Output**    | "say" block      | `printf()`                          |
| **Input**     | "ask" block      | `get_int()`, `get_string()`         |
| **If**        | "if" block       | `if (condition) { }`                |
| **Loop**      | "repeat" block   | `for`, `while`, `do-while`          |
| **Function**  | "define" block   | Function with prototype             |
| **Equals**    | `=`              | `==` (comparison), `=` (assignment) |

---

## 💭 Important Reminders

1. **Compile before running**: Always `make` before `./`
2. **Semicolons matter**: Every statement needs one
3. **Start counting at 0**: It's the convention in CS
4. **Use meaningful names**: `counter` not `c`, `age` not `a`
5. **Comment your code**: Future you will thank you
6. **Test edge cases**: What if user enters 0? Negative numbers?
7. **Don't repeat yourself**: If you're copying code, use a loop or function
8. **Check types carefully**: int division vs float division behaves differently

---

## 🎓 Learning Tips

### When Stuck

1. **Read error messages carefully** - They tell you exactly what's wrong
2. **Check syntax** - Missing semicolon? Wrong brackets?
3. **Print debug values** - Use `printf()` to see variable values
4. **Talk through your code** - Explain it out loud
5. **Take a break** - Fresh eyes help
6. **Use CS50 Duck** - The AI helper for this course

### Building Good Habits

- Write pseudocode first
- Test frequently (after every small change)
- Use consistent indentation
- Add comments for complex logic
- Start simple, then improve

---

_From Scratch to C - The journey begins! Every expert programmer was once where you are now. Keep practicing!_
