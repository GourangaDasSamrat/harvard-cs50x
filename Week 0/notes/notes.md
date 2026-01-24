# CS50 Lecture 0 - Introduction to Computer Science

## Personal Learning Notes

---

## 🎯 Key Takeaways

- **Computer Science = Problem Solving**: It's about taking inputs and creating outputs through algorithms
- **AI is a tool, not a replacement**: Understanding fundamentals empowers us to be drivers and creators, not just users
- **Programming building blocks are universal**: Functions, conditionals, loops, and variables appear in every language

---

## 💡 Core Concepts

### 1. **Binary & How Computers Think**

**Binary (Base-2)**: Computers only understand 0s and 1s (off/on)

- 1 bit = one binary digit (0 or 1)
- 1 byte = 8 bits
- Each position represents a power of 2

**Example**: Counting with 3 light bulbs

```
Position values: 4  2  1
Binary for 5:    1  0  1  (4 + 0 + 1 = 5)
Binary for 7:    1  1  1  (4 + 2 + 1 = 7)
```

**8-bit byte example**:

```
128  64  32  16  8  4  2  1
  0   0   0   0  0  1  0  1  = 5
```

### 2. **How Computers Represent Information**

#### ASCII (Text)

- Maps letters to numbers
- 'A' = 65 = `01000001` in binary
- 'H', 'I', '!' = 72, 73, 33

#### Unicode (Modern Text)

- Extends ASCII to include more characters
- Supports emoji and international languages
- Same numbers can mean different things based on context

#### RGB (Color)

- Red, Green, Blue values (0-255 each)
- Example: 72, 73, 33 = light yellow
- Images = collections of RGB pixels

#### Media

- **Videos**: Sequences of images (like a flipbook)
- **Music**: Represented using combinations of bytes

---

## 🔄 Algorithms

**Definition**: Step-by-step instructions to solve a problem

### Phone Book Example (Finding a Name)

**Three approaches with different efficiency**:

1. **Linear search** - Page by page
   - Big-O: `n` (worst case: check all n pages)

2. **Two pages at a time**
   - Big-O: `n/2` (twice as fast)

3. **Divide and conquer** (Binary search)
   - Go to middle, ask "left or right?", repeat
   - Big-O: `log₂(n)` (most efficient!)
   - Doubling the problem only adds one more step

**Key insight**: Algorithm choice matters! The third approach is exponentially faster for large datasets.

---

## 📝 Pseudocode

**Why pseudocode?**

1. Think through logic before coding
2. Communicate ideas to others
3. Language-independent problem solving

**Example** (Phone book binary search):

```
1  Pick up phone book
2  Open to middle of phone book
3  Look at page
4  If person is on page
5      Call person
6  Else if person is earlier in book
7      Open to middle of left half of book
8      Go back to line 3
9  Else if person is later in book
10     Open to middle of right half of book
11     Go back to line 3
12 Else
13     Quit
```

**Building blocks identified**:

- **Functions**: verbs (pick up, open, look at, call)
- **Conditionals**: if, else if, else
- **Boolean expressions**: true/false statements (person is on page)
- **Loops**: "go back to line 3"

---

## 🧱 Programming Building Blocks

### 1. Functions

- Reusable pieces of code
- Take inputs, produce outputs
- Example: `say("hello, world")`

### 2. Variables

- Store information
- Example: `answer` stores user's name

### 3. Conditionals

- Make decisions
- `if`, `else if`, `else`

### 4. Loops

- Repeat actions
- `repeat 3`, `forever`

### 5. Boolean Expressions

- True/false questions
- Example: `touching mouse-pointer?`

---

## 🎨 Scratch Programming

**What is Scratch?**

- Visual programming language by MIT
- Drag-and-drop blocks
- Great for learning programming concepts without syntax worries

### Scratch Coordinate System

- Center of stage: (0, 0)
- Helps position sprites

### Basic Scratch Program Pattern

```
when [green flag] clicked
    ask [What's your name?] and wait
    say (join [hello, ] (answer))
```

**Concept**: Input → Process → Output

- Input: User's name
- Process: Join "hello," with name
- Output: Cat says greeting

---

## 🔁 Abstraction

**Definition**: Simplifying problems by breaking them into smaller pieces

### Example 1: Repetitive Code

**Before** (repetitive):

```
play sound Meow until done
wait 1 seconds
play sound Meow until done
wait 1 seconds
play sound Meow until done
```

**After** (abstracted with loop):

```
repeat 3
    play sound Meow until done
    wait 1 seconds
```

### Example 2: Custom Functions

**Define your own block**:

```
define meow n times
    repeat n
        play sound meow until done
        wait 1 seconds
```

**Use it**:

```
when clicked
    meow 3 times
```

**Benefits**:

- Less repetitive code
- Easier to read and maintain
- Can reuse in multiple places

---

## 🎮 Practical Examples from Lecture

### Oscartime Game Concepts

- **Sprite costumes**: Change appearance based on conditions
- **Random positioning**: `pick random 0 to 240`
- **Gravity simulation**: `change y by -1` (falling)
- **Collision detection**: `if touching Oscar?`
- **Custom functions**: `go to top` abstraction

### Ivy's Hardest Game Concepts

- **Keyboard input**: Detect arrow keys
- **Wall collision**: Prevent sprite from going off-screen
- **Sprite interaction**: Multiple sprites interacting
- **AI following**: Make sprite follow another sprite

---

## 💻 Introduction to AI in Programming

### Simple Chatbot Example (chat.py)

```python
from openai import OpenAI

client = OpenAI()
user_prompt = input("Prompt: ")
system_prompt = "Limit your answer to one sentence. Pretend you're a cat."

response = client.responses.create(
    input=user_prompt,
    instructions=system_prompt,
    model="gpt-5"
)

print(response.output_text)
```

**Key concepts**:

- Import libraries for additional capabilities
- Get user input
- Send request to AI model
- Display response

**Academic Honesty**: Only use CS50 Duck (cs50.ai) for AI help in this course

---

## 🎓 Learning Strategies

### What I Need to Remember

1. **Start with pseudocode** before writing actual code
2. **Look for patterns** - repetition often means there's a better way
3. **Break down problems** - abstraction is key
4. **Trial and error is normal** - programming is iterative
5. **When stuck**, talk through the problem:
   - What specific problem am I solving?
   - What's working?
   - What's not working?

### Course Progression

- Week 0: Scratch (visual programming)
- Future weeks: C, Python, and more
- Each language builds on the same fundamental concepts

---

## 📌 Important Terms Glossary

- **Bit**: Binary digit (0 or 1)
- **Byte**: 8 bits
- **ASCII**: Character encoding standard
- **Unicode**: Extended character encoding (includes emoji)
- **RGB**: Red, Green, Blue color system
- **Algorithm**: Step-by-step problem-solving instructions
- **Big-O notation**: Way to describe algorithm efficiency
- **Pseudocode**: Human-readable algorithm description
- **IDE**: Integrated Development Environment (like VS Code)
- **Abstraction**: Simplifying complex problems
- **Sprite**: Character/object in Scratch
- **Boolean**: True or false value

---

## ✅ Self-Check Questions

1. Can I explain how binary represents numbers?
2. Do I understand why algorithm choice matters?
3. Can I write basic pseudocode for a simple problem?
4. Do I recognize the four main programming building blocks?
5. Can I identify when to use abstraction in my code?

---

## 🚀 Next Steps

- [ ] Practice with Scratch programming
- [ ] Create pseudocode for everyday problems
- [ ] Experiment with loops and conditionals
- [ ] Build something in Scratch using all building blocks
- [ ] Review binary/ASCII/RGB concepts

---

_Remember: Programming is problem-solving. Every expert was once a beginner!_
