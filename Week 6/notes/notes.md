# CS50 Lecture 6 - Python
## Personal Learning Notes

---

## 🎯 Key Takeaways

- **Python is higher-level than C** - More abstraction, less manual work
- **Interpreted, not compiled** - Run directly without separate compilation
- **Much simpler syntax** - No semicolons, curly braces, or type declarations
- **Trade-off: Speed vs ease** - Python is slower but easier to write
- **Built-in data structures** - Lists, dictionaries, sets come standard
- **Learning to learn** - Skills transfer across programming languages

---

## 🐍 Why Python?

### C vs Python

**C (compiled language)**:
```c
#include <stdio.h>

int main(void)
{
    printf("hello, world\n");
}
```

**Python (interpreted language)**:
```python
print("hello, world")
```

**Differences**:
- ✅ No `#include` statements
- ✅ No `main()` function required
- ✅ No semicolons
- ✅ No compilation step
- ✅ Just run: `python hello.py`

### The Trade-off

**Python advantages**:
- Faster to write code
- Easier to read
- Less boilerplate
- Built-in memory management
- Huge library ecosystem

**C advantages**:
- Faster execution
- More control over memory
- Better for system programming
- Smaller runtime overhead

---

## 🚀 Getting Started

### Running Python

**No compilation needed!**

```bash
python hello.py  # Just run it directly
```

**In C**:
```bash
make hello  # Compile first
./hello     # Then run
```

### Your First Python Program

```python
# A program that says hello to the world
print("hello, world")
```

**Key points**:
- Comments use `#` (not `//`)
- No semicolons needed
- No type declarations
- `print()` automatically adds newline

---

## 📝 Variables

### No Type Declaration!

**C**:
```c
int counter = 0;
string name = "Alice";
```

**Python**:
```python
counter = 0
name = "Alice"
```

**Python infers the type automatically!**

### Variable Operations

```python
# Incrementing
counter = 0
counter = counter + 1  # ✅ Works
counter += 1           # ✅ Preferred
counter++              # ❌ Doesn't exist in Python!

# Other operations
x = 10
x -= 5   # Subtraction
x *= 2   # Multiplication
x /= 2   # Division
x //= 2  # Floor division
```

---

## 📊 Data Types

### Basic Types

```python
# No type declaration needed
x = 5           # int
y = 3.14        # float
name = "Alice"  # str (string)
is_valid = True # bool (True/False, capitalized!)
```

**Note**: `bool` values are `True` and `False` (capitalized!)

### Python Types

| Python Type | Similar to C | Example |
|-------------|--------------|---------|
| `int` | `int`, `long` | `42`, `-10` |
| `float` | `float`, `double` | `3.14`, `-0.5` |
| `str` | `string` | `"hello"` |
| `bool` | `bool` | `True`, `False` |
| `list` | arrays | `[1, 2, 3]` |
| `dict` | struct (kind of) | `{"name": "Alice"}` |
| `set` | - | `{1, 2, 3}` |
| `tuple` | - | `(1, 2, 3)` |

**No `char` in Python** - use single-character strings instead!

---

## 🔤 Strings

### String Basics

```python
# Creating strings
name = "Alice"
greeting = 'Hello'  # Single or double quotes work

# String concatenation
message = "Hello, " + name
print(message)  # Hello, Alice
```

### String Methods

**Python strings have built-in methods!**

```python
text = "Hello World"

text.upper()      # "HELLO WORLD"
text.lower()      # "hello world"
text.capitalize() # "Hello world"
text.strip()      # Removes whitespace
text.split()      # Splits into list: ["Hello", "World"]
```

### F-Strings (Formatted Strings)

**The modern way** to format strings:

```python
name = "Alice"
age = 25

# F-string (requires 'f' prefix)
print(f"Hello, {name}!")              # Hello, Alice!
print(f"{name} is {age} years old")   # Alice is 25 years old

# Can include expressions
print(f"Next year: {age + 1}")        # Next year: 26
```

**Old ways** (still work):
```python
# Concatenation
print("Hello, " + name)

# Comma separation (adds space automatically)
print("Hello,", name)
```

---

## 📥 Getting Input

### Using CS50 Library

```python
from cs50 import get_int, get_string, get_float

name = get_string("What's your name? ")
age = get_int("What's your age? ")
gpa = get_float("What's your GPA? ")

print(f"Hello, {name}!")
```

### Using Built-in `input()`

```python
# input() always returns a string!
name = input("What's your name? ")
print(f"Hello, {name}")

# Convert to int or float if needed
age = int(input("What's your age? "))
price = float(input("Enter price: "))
```

**Important**: `input()` always returns a string - convert if needed!

---

## ➕ Calculator Example

### Basic Calculator

```python
# Using CS50 library
from cs50 import get_int

x = get_int("x: ")
y = get_int("y: ")

print(x + y)
```

### Without CS50 Library

```python
# Using built-in input (must convert!)
x = int(input("x: "))
y = int(input("y: "))

print(x + y)
```

**Common mistake**:
```python
# WRONG - concatenates strings!
x = input("x: ")  # "5"
y = input("y: ")  # "3"
print(x + y)      # "53" not 8!

# CORRECT - convert to int
x = int(input("x: "))
y = int(input("y: "))
print(x + y)      # 8
```

---

## 🔀 Conditionals

### Basic If Statements

**C**:
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

**Python**:
```python
if x < y:
    print("x is less than y")
elif x > y:
    print("x is greater than y")
else:
    print("x is equal to y")
```

**Key differences**:
- ✅ No parentheses needed (though allowed)
- ✅ Colon `:` instead of `{`
- ✅ Indentation instead of `}`
- ✅ `elif` instead of `else if`

### Logical Operators

**C**:
```c
if (c == 'Y' || c == 'y')
{
    printf("Agreed.\n");
}
```

**Python**:
```python
if s == "Y" or s == "y":
    print("Agreed.")
```

**Operators**:
- `and` instead of `&&`
- `or` instead of `||`
- `not` instead of `!`

### Using Lists for Multiple Values

```python
s = input("Do you agree? ")

if s in ["y", "yes", "Y", "Yes"]:
    print("Agreed.")
else:
    print("Not agreed.")
```

**The `in` keyword** checks if value is in list/string!

---

## 🔁 Loops

### For Loops

**C**:
```c
for (int i = 0; i < 3; i++)
{
    printf("meow\n");
}
```

**Python**:
```python
for i in range(3):
    print("meow")
```

**Key points**:
- `range(3)` generates 0, 1, 2 (stops before 3)
- No need to declare `i`
- Indentation defines loop body

### Range Variations

```python
range(3)        # 0, 1, 2
range(1, 4)     # 1, 2, 3 (start, stop)
range(0, 10, 2) # 0, 2, 4, 6, 8 (start, stop, step)
```

### While Loops

**Python**:
```python
i = 0
while i < 3:
    print("meow")
    i += 1
```

### Forever Loops

```python
while True:
    print("This runs forever!")
    # Use Ctrl+C to stop
```

### Looping Over Strings

```python
text = "Hello"

for c in text:
    print(c.upper())
# H
# E
# L
# L
# O
```

---

## 🎨 Print Function Features

### Multiple Arguments

```python
# Comma automatically adds spaces
print("Hello", "world")  # Hello world

# Can print multiple values
name = "Alice"
age = 25
print("Name:", name, "Age:", age)  # Name: Alice Age: 25
```

### Named Parameters

```python
# end parameter (default is '\n')
print("Hello", end="")
print("World")  # HelloWorld (no newline between)

# sep parameter (default is ' ')
print("a", "b", "c", sep="-")  # a-b-c
```

### String Multiplication

```python
print("?" * 4)     # ????
print("#" * 10)    # ##########
print("meow\n" * 3) # meow three times
```

---

## 📚 Lists (Arrays in Python)

### Creating Lists

```python
# Empty list
scores = []

# List with values
scores = [72, 73, 33]

# Mixed types (allowed in Python!)
mixed = [1, "hello", 3.14, True]
```

### Accessing Elements

```python
scores = [72, 73, 33]

print(scores[0])  # 72 (first element)
print(scores[1])  # 73
print(scores[-1]) # 33 (last element!)
print(scores[-2]) # 73 (second to last)
```

**Negative indexing** - counts from the end!

### List Methods

```python
scores = []

# Add elements
scores.append(95)      # Add to end: [95]
scores.append(87)      # [95, 87]
scores.insert(0, 100)  # Insert at index 0: [100, 95, 87]

# Remove elements
scores.remove(95)      # Remove first 95
scores.pop()           # Remove and return last element
scores.pop(0)          # Remove and return element at index 0

# Other useful methods
scores.sort()          # Sort in place
scores.reverse()       # Reverse in place
len(scores)            # Get length
sum(scores)            # Get sum
```

### Iterating Over Lists

```python
scores = [95, 87, 92]

# Method 1: Direct iteration
for score in scores:
    print(score)

# Method 2: With index
for i in range(len(scores)):
    print(f"Score {i}: {scores[i]}")
```

### Example: Calculate Average

```python
scores = [72, 73, 33]

average = sum(scores) / len(scores)
print(f"Average: {average}")  # 59.333...
```

---

## 📖 Dictionaries (Hash Tables)

### What Are Dictionaries?

**Dictionaries** store key-value pairs (like hash tables in C)

```python
# Dictionary with curly braces
person = {
    "name": "Alice",
    "age": 25,
    "city": "Boston"
}
```

### Accessing Values

```python
person = {"name": "Alice", "age": 25}

# Access by key
print(person["name"])  # Alice
print(person["age"])   # 25

# Using get (safer - returns None if key doesn't exist)
print(person.get("name"))     # Alice
print(person.get("height"))   # None (no error!)
```

### Phone Book Example

**Simple dictionary**:
```python
people = {
    "Kelly": "+1-617-495-1000",
    "David": "+1-617-495-1000",
    "John": "+1-949-468-2750"
}

name = input("Name: ")

if name in people:
    print(f"Number: {people[name]}")
else:
    print("Not found")
```

**List of dictionaries**:
```python
people = [
    {"name": "Kelly", "number": "+1-617-495-1000"},
    {"name": "David", "number": "+1-617-495-1000"},
    {"name": "John", "number": "+1-949-468-2750"}
]

name = input("Name: ")

for person in people:
    if person["name"] == name:
        print(f"Found {person['number']}")
        break
else:  # Executes if loop completes without break
    print("Not found")
```

---

## 🎯 Functions

### Defining Functions

```python
def meow():
    print("meow")

# Call the function
meow()  # meow
```

### Functions with Parameters

```python
def meow(n):
    for i in range(n):
        print("meow")

meow(3)  # meows 3 times
```

### Functions with Return Values

```python
def square(n):
    return n * n

result = square(5)
print(result)  # 25
```

### Main Function Convention

```python
def main():
    name = input("Name: ")
    hello(name)

def hello(name):
    print(f"Hello, {name}")

# Call main at the end
main()
```

**Why?** By convention, helps organize code!

---

## ⚠️ Exception Handling

### Try-Except Blocks

**Problem**: User inputs wrong type

```python
# Without exception handling - crashes!
n = int(input("Number: "))  # User types "cat" - ERROR!
```

**Solution**: Catch the exception

```python
try:
    n = int(input("Number: "))
    print("Integer.")
except ValueError:
    print("Not an integer.")
```

### Getting Valid Input

```python
while True:
    try:
        n = int(input("Number: "))
        if n > 0:
            break
    except ValueError:
        print("Not an integer.")

print(f"You entered: {n}")
```

---

## 💻 Command-Line Arguments

### Using sys Module

```python
from sys import argv

if len(argv) == 2:
    print(f"hello, {argv[1]}")
else:
    print("hello, world")
```

**Running**:
```bash
python greet.py Alice  # hello, Alice
python greet.py         # hello, world
```

**argv structure**:
- `argv[0]` - Program name (`greet.py`)
- `argv[1]` - First argument
- `argv[2]` - Second argument
- etc.

### Exit Status

```python
import sys

if len(sys.argv) != 2:
    print("Missing command-line argument")
    sys.exit(1)  # Exit with error code

print(f"hello, {sys.argv[1]}")
sys.exit(0)  # Exit successfully
```

---

## 📄 CSV Files

### Writing CSV Files

```python
import csv

name = input("Name: ")
number = input("Number: ")

# Open and write
with open("phonebook.csv", "a") as file:
    writer = csv.writer(file)
    writer.writerow([name, number])
```

**`with` statement**:
- Automatically closes file when done
- Best practice!

### Writing Dictionaries to CSV

```python
import csv

name = input("Name: ")
number = input("Number: ")

with open("phonebook.csv", "a") as file:
    writer = csv.DictWriter(file, fieldnames=["name", "number"])
    writer.writerow({"name": name, "number": number})
```

### Reading CSV Files

```python
import csv

with open("phonebook.csv", "r") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(f"{row['name']}: {row['number']}")
```

---

## 🎭 Object-Oriented Programming

### Objects and Methods

**Objects** in Python have both:
- **Attributes** (data)
- **Methods** (functions)

**Example with strings**:
```python
name = "alice"

# Methods (functions that belong to the object)
print(name.upper())       # ALICE
print(name.capitalize())  # Alice
print(name.replace("a", "A"))  # Alice

# Chaining methods
print(name.upper().replace("A", "@"))  # @LICE
```

### Example: String Methods

```python
text = input("Enter text: ").lower()

# All of these are methods
if text in ["y", "yes"]:
    print("Agreed")
```

**`.lower()`** is a method of string objects!

---

## 🔢 Floating Point & Division

### No Truncation in Python!

**C**:
```c
int x = 1;
int y = 3;
printf("%i\n", x / y);  // 0 (truncated!)
```

**Python**:
```python
x = 1
y = 3
print(x / y)  # 0.3333333333333333 (no truncation!)
```

### Floor Division

```python
# Regular division
print(7 / 2)   # 3.5

# Floor division (rounds down)
print(7 // 2)  # 3
```

### Floating Point Imprecision

**Still exists in Python!**

```python
x = 1
y = 3
z = x / y

# Show 50 decimal places
print(f"{z:.50f}")
# 0.33333333333333331482961625624739099293947219848633
```

---

## 🏗️ Python vs C - Side by Side

### Hello World

| C | Python |
|---|--------|
| `#include <stdio.h>`<br>`int main(void) {`<br>&nbsp;&nbsp;`printf("hello\n");`<br>`}` | `print("hello")` |

### Variables

| C | Python |
|---|--------|
| `int x = 5;` | `x = 5` |
| `string s = "hi";` | `s = "hi"` |

### Conditionals

| C | Python |
|---|--------|
| `if (x < y) {`<br>&nbsp;&nbsp;`...`<br>`}` | `if x < y:`<br>&nbsp;&nbsp;`...` |
| `else if` | `elif` |
| `&&` | `and` |
| `\|\|` | `or` |
| `!` | `not` |

### Loops

| C | Python |
|---|--------|
| `for (int i = 0; i < 3; i++)` | `for i in range(3):` |
| `while (condition)` | `while condition:` |

### Arrays/Lists

| C | Python |
|---|--------|
| `int scores[3];` | `scores = []` |
| `scores[0] = 95;` | `scores.append(95)` |

---

## 💡 Important Python Conventions

### Indentation Matters!

**WRONG** ❌:
```python
if x > 0:
print("positive")  # IndentationError!
```

**CORRECT** ✅:
```python
if x > 0:
    print("positive")  # 4 spaces (standard)
```

**Use 4 spaces** for indentation (standard in Python)

### No Semicolons!

```python
# WRONG (works but non-idiomatic)
print("hello");

# CORRECT
print("hello")
```

### Boolean Values

```python
# Capitalized!
is_valid = True
is_empty = False

# NOT lowercase (that's C/Java)
# is_valid = true  # NameError!
```

### Comments

```python
# Single line comment

"""
Multi-line comment
(actually a docstring)
"""
```

---

## 📦 Libraries and Modules

### Importing

```python
# Import entire module
import csv
csv.writer(file)

# Import specific items
from csv import writer, DictWriter
writer(file)

# Import with alias
import csv as c
c.writer(file)

# Import everything (not recommended)
from csv import *
writer(file)
```

### Common Libraries

**Built-in**:
- `sys` - System-specific parameters
- `csv` - CSV file handling
- `random` - Random number generation
- `math` - Mathematical functions

**CS50**:
- `cs50` - Get functions (get_int, get_string, etc.)

**Third-party** (install with `pip`):
```bash
pip install pillow  # Image processing
pip install requests  # HTTP requests
```

---

## ⚠️ Common Mistakes

### 1. Forgetting Colons

```python
# WRONG
if x > 0
    print("positive")

# CORRECT
if x > 0:
    print("positive")
```

### 2. Wrong Indentation

```python
# WRONG - inconsistent indentation
if x > 0:
  print("positive")
    print("still positive")  # IndentationError!

# CORRECT
if x > 0:
    print("positive")
    print("still positive")
```

### 3. input() Returns String

```python
# WRONG
age = input("Age: ")
print(age + 5)  # TypeError!

# CORRECT
age = int(input("Age: "))
print(age + 5)
```

### 4. Using = Instead of ==

```python
# WRONG (assignment, not comparison!)
if x = 5:
    print("five")

# CORRECT
if x == 5:
    print("five")
```

### 5. True/False Capitalization

```python
# WRONG
if is_valid == true:  # NameError!

# CORRECT
if is_valid == True:
    # Or just:
if is_valid:
```

---

## ✅ Self-Check Questions

1. Can I write a simple Python program without looking at notes?
2. Do I understand when to use `int(input())` vs just `input()`?
3. Can I create and manipulate lists?
4. Do I know the difference between lists and dictionaries?
5. Can I use try-except to handle errors?
6. Do I understand f-strings for formatting?
7. Can I write functions with parameters and return values?
8. Do I remember that indentation matters in Python?

---

## 🚀 Practice Exercises

### Beginner
- [ ] Convert a C program to Python
- [ ] Create a simple calculator
- [ ] Build a list and calculate its average
- [ ] Use try-except to validate user input
- [ ] Create a simple phonebook with dictionaries

### Intermediate
- [ ] Read and write CSV files
- [ ] Implement linear search in Python
- [ ] Create a program with command-line arguments
- [ ] Build a simple grade book (list of dictionaries)
- [ ] Handle multiple types of exceptions

### Advanced
- [ ] Create a spell checker using dictionaries
- [ ] Implement a simple database with CSV
- [ ] Build a text analyzer (word count, frequency)
- [ ] Create a command-line tool with multiple features
- [ ] Use third-party libraries (requests, Pillow)

---

## 📌 Important Reminders

1. **No semicolons** in Python
2. **Indentation matters** - use 4 spaces
3. **Colons after** if, elif, else, for, while, def
4. **No type declarations** - Python infers types
5. **input() returns string** - convert if needed
6. **True/False** are capitalized
7. **Use f-strings** for formatting
8. **Lists use []**, dictionaries use {}
9. **in keyword** checks membership
10. **Python is slower** but easier to write

---

*Python builds on everything you learned in C. The logic is the same - just the syntax is friendlier! Focus on understanding the concepts, not memorizing syntax.*