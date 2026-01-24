# CS50 Lecture 3 - Algorithms

## Personal Learning Notes

---

## 🎯 Key Takeaways

- **Algorithms have measurable efficiency** - Some are fast, some are slow
- **Big O notation** describes worst-case performance as input grows
- **Binary search is WAY faster** than linear search (but needs sorted data)
- **Structs let us create custom data types** - Group related data together
- **Sorting enables better searching** - Sorted data unlocks efficient algorithms
- **Recursion** is when a function calls itself - powerful but needs a base case

---

## 🔍 Searching Algorithms

### Linear Search

**Concept**: Check each element one by one from start to finish

**Real-world analogy**: Looking through every locker one at a time to find number 50

**Pseudocode**:

```
For each door from left to right
    If 50 is behind door
        Return true
Return false
```

**In code notation**:

```
For i from 0 to n-1
    If 50 is behind doors[i]
        Return true
Return false
```

**C Implementation** (searching for integers):

```c
#include <cs50.h>
#include <stdio.h>

int main(void)
{
    // An array of integers
    int numbers[] = {20, 500, 10, 5, 100, 1, 50};

    // Search for number
    int n = get_int("Number: ");
    for (int i = 0; i < 7; i++)
    {
        if (numbers[i] == n)
        {
            printf("Found\n");
            return 0;  // Success!
        }
    }
    printf("Not found\n");
    return 1;  // Failure
}
```

**Searching for strings**:

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

int main(void)
{
    string strings[] = {"battleship", "boot", "cannon", "iron", "thimble"};

    string s = get_string("String: ");
    for (int i = 0; i < 5; i++)
    {
        if (strcmp(strings[i], s) == 0)  // strcmp returns 0 if equal
        {
            printf("Found\n");
            return 0;
        }
    }
    printf("Not found\n");
    return 1;
}
```

**Key points**:

- Cannot use `==` to compare strings!
- Must use `strcmp()` from `string.h`
- `strcmp()` returns `0` when strings match
- Simple but slow for large datasets

---

### Binary Search

**Concept**: Divide and conquer - keep halving the search space

**Requirements**: Array MUST be sorted first!

**Real-world analogy**: Opening a phone book to the middle, then going left or right based on alphabetical order

**Pseudocode**:

```
If no doors left
    Return false
If 50 is behind middle door
    Return true
Else if 50 < middle door
    Search left half
Else if 50 > middle door
    Search right half
```

**More code-like**:

```
If no doors left
    Return false
If 50 is behind doors[middle]
    Return true
Else if 50 < doors[middle]
    Search doors[0] through doors[middle - 1]
Else if 50 > doors[middle]
    Search doors[middle + 1] through doors[n - 1]
```

**Why it's faster**: Each step eliminates HALF the remaining data!

---

## 📊 Big O Notation - Measuring Efficiency

### What is Big O?

**Big O** describes the **worst-case** running time of an algorithm as input size grows

**Think of it as**: "In the worst possible scenario, how does runtime grow when I double the data?"

### Common Running Times (Fastest to Slowest)

```
O(1)        - Constant time      - FASTEST
O(log n)    - Logarithmic time   - Very fast
O(n)        - Linear time        - Decent
O(n log n)  - Linearithmic time  - Good
O(n²)       - Quadratic time     - SLOW
```

**Visual representation**:

```
       |                                    O(n²)
Time   |                              O(n log n)
  to   |                         O(n)
Solve  |                   O(log n)
       |             O(1)
       |________________________
            Size of Problem
```

### Examples

**O(1)** - Constant time:

- Accessing array element: `array[5]`
- No matter how big the array, same speed!

**O(log n)** - Logarithmic:

- Binary search
- Each step cuts problem in half
- 1000 items? Only ~10 steps max!

**O(n)** - Linear:

- Linear search
- Checking every element once
- 1000 items? Up to 1000 steps

**O(n²)** - Quadratic:

- Selection sort, Bubble sort
- Nested loops checking everything
- 1000 items? Up to 1,000,000 steps!

**O(n log n)** - Linearithmic:

- Merge sort
- Best we can do for comparison-based sorting
- 1000 items? ~10,000 steps

---

## 📈 Other Important Notations

### Omega (Ω) - Best Case

**Omega** describes the **best-case** running time

**Examples**:

- Linear search: `Ω(1)` - might find it immediately!
- Binary search: `Ω(1)` - might be in the middle!
- Bubble sort: `Ω(n)` - if already sorted, just verify once

### Theta (Θ) - Exact Case

**Theta** means upper and lower bounds are the SAME

**Examples**:

- Selection sort: `Θ(n²)` - always same speed, sorted or not
- Merge sort: `Θ(n log n)` - always same process

---

## 🧩 Structs - Custom Data Types

### Why Structs?

**Problem**: Related data in separate arrays gets messy

```c
// BAD: Two parallel arrays
string names[] = {"Kelly", "David", "John"};
string numbers[] = {"+1-617-495-1000", "+1-617-495-1000", "+1-949-468-2750"};
// What if they get out of sync? 😱
```

**Solution**: Group related data together!

### Creating a Struct

```c
// Define a new data type called 'person'
typedef struct
{
    string name;
    string number;
}
person;  // This is the name of our new type
```

**Breaking it down**:

- `typedef struct` - "I'm defining a new type"
- Inside `{ }` - The properties/fields
- `person` at the end - Name of the new type

### Using Structs

```c
#include <cs50.h>
#include <stdio.h>
#include <string.h>

typedef struct
{
    string name;
    string number;
}
person;

int main(void)
{
    // Create an array of 3 people
    person people[3];

    // Set values using dot notation
    people[0].name = "Kelly";
    people[0].number = "+1-617-495-1000";

    people[1].name = "David";
    people[1].number = "+1-617-495-1000";

    people[2].name = "John";
    people[2].number = "+1-949-468-2750";

    // Search for name
    string name = get_string("Name: ");
    for (int i = 0; i < 3; i++)
    {
        if (strcmp(people[i].name, name) == 0)
        {
            printf("Found %s\n", people[i].number);
            return 0;
        }
    }
    printf("Not found\n");
    return 1;
}
```

**Key syntax**: `people[i].name` - Use dot (`.`) to access struct fields

**Benefits**:

- Data stays organized
- Easier to understand
- Less prone to errors
- Can pass whole person to functions

---

## 🔄 Sorting Algorithms

### Why Sort?

**Sorting unlocks faster algorithms!**

- Sorted data → Can use binary search (O(log n))
- Unsorted data → Stuck with linear search (O(n))

---

### Selection Sort

**Concept**: Find the smallest, swap it to the front, repeat

**How it works**:

```
Step 1: [6, 3, 4, 1] → Find smallest (1), swap with first
        [1, 3, 4, 6]

Step 2: [1, 3, 4, 6] → Find smallest in rest (3), already in place
        [1, 3, 4, 6]

Step 3: [1, 3, 4, 6] → Find smallest in rest (4), already in place
        [1, 3, 4, 6]

Done!
```

**Pseudocode**:

```
For i from 0 to n–1
    Find smallest number between numbers[i] and numbers[n-1]
    Swap smallest number with numbers[i]
```

**Analysis**:

- First pass: n-1 comparisons
- Second pass: n-2 comparisons
- Total: (n-1) + (n-2) + ... + 1 = n(n-1)/2

**Efficiency**:

- Worst case: `O(n²)`
- Best case: `Ω(n²)` (always checks everything!)
- Therefore: `Θ(n²)` (same in all cases)

**Characteristics**:

- Simple to understand
- Slow for large datasets
- Doesn't improve if already sorted

---

### Bubble Sort

**Concept**: Repeatedly swap adjacent elements that are out of order

**How it works**:

```
Pass 1: [6, 3, 4, 1]
        [3, 6, 4, 1]  (swap 6,3)
        [3, 4, 6, 1]  (swap 6,4)
        [3, 4, 1, 6]  (swap 6,1) - largest bubbled to end!

Pass 2: [3, 4, 1, 6]
        [3, 4, 1, 6]  (no swap)
        [3, 1, 4, 6]  (swap 4,1)
        [3, 1, 4, 6]  (6 already in place)

Pass 3: [3, 1, 4, 6]
        [1, 3, 4, 6]  (swap 3,1)
        Done!
```

**Pseudocode**:

```
Repeat n-1 times
    For i from 0 to n–2
        If numbers[i] and numbers[i+1] out of order
            Swap them
    If no swaps
        Quit
```

**Analysis**:

- (n-1) × (n-1) = n² - 2n + 1

**Efficiency**:

- Worst case: `O(n²)` (reversed array)
- Best case: `Ω(n)` (already sorted, just verify!)
- Can optimize by quitting early if no swaps

**Characteristics**:

- Simple to implement
- Can detect if already sorted
- Still slow for large datasets

---

### Merge Sort

**Concept**: Divide, conquer, and merge (uses recursion!)

**How it works**:

```
Original: [6, 3, 4, 1]

Split:    [6, 3] | [4, 1]
Split:    [6] [3] | [4] [1]

Merge:    [3, 6] | [1, 4]
Merge:    [1, 3, 4, 6]

Done!
```

**Detailed walkthrough**:

```
[6, 3, 4, 1]
    ↓
[6, 3] [4, 1]      Split in half
    ↓       ↓
[6] [3] [4] [1]    Split until single elements
    ↓       ↓
[3, 6] [1, 4]      Merge sorted pairs
    ↓
[1, 3, 4, 6]       Merge sorted halves
```

**Pseudocode**:

```
If only one number
    Quit
Else
    Sort left half of numbers
    Sort right half of numbers
    Merge sorted halves
```

**Analysis**:

- Each level divides by 2: log₂(n) levels
- Each level does n comparisons
- Total: n × log₂(n)

**Efficiency**:

- Worst case: `O(n log n)`
- Best case: `Ω(n log n)`
- Therefore: `Θ(n log n)` (consistent!)

**Characteristics**:

- MUCH faster for large datasets
- More complex to implement
- Uses extra memory for merging
- Industry standard for general sorting

---

## 🔄 Recursion

### What is Recursion?

**Recursion**: When a function calls itself

**Two essential parts**:

1. **Base case** - Condition to STOP (prevents infinite loop)
2. **Recursive case** - Function calls itself with smaller problem

### Simple Example: Drawing a Pyramid

**Iterative approach** (using loops):

```c
void draw(int n)
{
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < i + 1; j++)
        {
            printf("#");
        }
        printf("\n");
    }
}
```

**Recursive approach**:

```c
void draw(int n)
{
    // BASE CASE: Stop if nothing to draw
    if (n <= 0)
    {
        return;
    }

    // RECURSIVE CASE: Draw smaller pyramid first
    draw(n - 1);

    // Then draw one more row
    for (int i = 0; i < n; i++)
    {
        printf("#");
    }
    printf("\n");
}
```

**How it works** (draw(3)):

```
draw(3)
  ↓ calls draw(2)
    ↓ calls draw(1)
      ↓ calls draw(0)
      ← returns (base case!)
      Prints: #
    ← returns
    Prints: ##
  ← returns
  Prints: ###

Output:
#
##
###
```

### Recursion in Binary Search

Remember binary search pseudocode:

```
If no doors left
    Return false
If 50 is behind middle door
    Return true
Else if 50 < middle door
    Search left half      ← Recursion!
Else if 50 > middle door
    Search right half     ← Recursion!
```

**The function calls itself** on a smaller portion of data!

### Recursion in Merge Sort

```
If only one number
    Quit                  ← Base case
Else
    Sort left half        ← Recursion!
    Sort right half       ← Recursion!
    Merge sorted halves
```

### Why Use Recursion?

**Pros**:

- Elegant and concise code
- Natural for divide-and-conquer problems
- Mirrors how we think about problems

**Cons**:

- Can be harder to understand at first
- Uses more memory (call stack)
- Risk of stack overflow if no base case

**Rule**: Always make sure you have a base case that will be reached!

---

## 📊 Algorithm Comparison Summary

| Algorithm          | Best Case  | Worst Case | Average    | Notes                     |
| ------------------ | ---------- | ---------- | ---------- | ------------------------- |
| **Linear Search**  | Ω(1)       | O(n)       | O(n)       | Simple, works on unsorted |
| **Binary Search**  | Ω(1)       | O(log n)   | O(log n)   | Fast, needs sorted data   |
| **Selection Sort** | Ω(n²)      | O(n²)      | Θ(n²)      | Always slow               |
| **Bubble Sort**    | Ω(n)       | O(n²)      | O(n²)      | Can optimize for sorted   |
| **Merge Sort**     | Ω(n log n) | O(n log n) | Θ(n log n) | Best for general use      |

---

## 💡 Key Concepts to Remember

### When to Use Each Search

**Linear Search**:

- ✅ Data is unsorted
- ✅ Small dataset
- ✅ Simple implementation needed

**Binary Search**:

- ✅ Data is sorted (or can be sorted once)
- ✅ Large dataset
- ✅ Speed is important

### When to Use Each Sort

**Bubble/Selection Sort**:

- Small datasets (< 100 items)
- Learning purposes
- Simple implementation needed

**Merge Sort**:

- Large datasets
- Speed matters
- Willing to use extra memory

### Return Values Convention

```c
return 0;  // Success
return 1;  // Failure/Error
```

This is a standard Unix/Linux convention!

---

## 🎯 Problem-Solving Patterns

### Searching Pattern

1. **Understand the data**: Sorted or unsorted?
2. **Choose algorithm**:
   - Unsorted → Linear search
   - Sorted → Binary search
3. **Implement**: Use appropriate comparison
4. **Return**: 0 for found, 1 for not found

### Sorting Pattern

1. **Analyze size**: How much data?
2. **Choose algorithm**:
   - Small/learning → Bubble/Selection
   - Production/large → Merge sort
3. **Implement**: Follow algorithm steps
4. **Verify**: Test with sorted/unsorted/reversed data

### Recursion Pattern

1. **Identify base case**: When to stop?
2. **Define recursive case**: How to make problem smaller?
3. **Ensure progress**: Each call moves toward base case
4. **Test thoroughly**: Easy to create infinite loops!

---

## 🔍 Common Mistakes & Tips

### Mistakes to Avoid

**1. String comparison with ==**

```c
// WRONG
if (strings[i] == s)

// CORRECT
if (strcmp(strings[i], s) == 0)
```

**2. Off-by-one errors in arrays**

```c
// If array has 5 elements
for (int i = 0; i < 5; i++)  // Correct: 0,1,2,3,4
for (int i = 0; i <= 5; i++) // WRONG: Goes to 5 (out of bounds!)
```

**3. Forgetting base case in recursion**

```c
void countdown(int n)
{
    printf("%i\n", n);
    countdown(n - 1);  // INFINITE! No base case!
}

// CORRECT
void countdown(int n)
{
    if (n <= 0) return;  // Base case!
    printf("%i\n", n);
    countdown(n - 1);
}
```

**4. Using binary search on unsorted data**

- Binary search REQUIRES sorted data!
- Sort first, then search

### Pro Tips

**Tip 1**: When in doubt about efficiency, think:

- Will doubling the input double the time? → O(n)
- Will doubling the input quadruple the time? → O(n²)
- Will doubling the input barely change time? → O(log n)

**Tip 2**: Draw it out!

- Visualize arrays, sorting steps, recursion trees
- Makes algorithms much clearer

**Tip 3**: Test edge cases:

- Empty array
- Single element
- Already sorted
- Reverse sorted
- Duplicates

---

## 📌 Important Terms Glossary

- **Algorithm**: Step-by-step instructions to solve a problem
- **Big O (O)**: Upper bound (worst case) running time
- **Omega (Ω)**: Lower bound (best case) running time
- **Theta (Θ)**: Tight bound (best = worst case)
- **Linear search**: Check each element sequentially
- **Binary search**: Divide and conquer on sorted data
- **Sorting**: Arranging data in order
- **Selection sort**: Find minimum, swap to front, repeat
- **Bubble sort**: Swap adjacent out-of-order pairs
- **Merge sort**: Divide, sort halves, merge back
- **Struct**: Custom data type grouping related data
- **Recursion**: Function calling itself
- **Base case**: Condition that stops recursion
- **Recursive case**: Recursive function call with smaller input
- **Asymptotic notation**: How algorithm scales with input size
- **strcmp()**: String comparison function (returns 0 if equal)

---

## ✅ Self-Check Questions

1. Can I explain why binary search requires sorted data?
2. Do I understand Big O vs Omega vs Theta?
3. Can I write linear search from scratch?
4. Do I know when to use structs?
5. Can I identify base case and recursive case?
6. Do I understand why merge sort is O(n log n)?
7. Can I compare sorting algorithms and their trade-offs?
8. Do I remember to use strcmp() for string comparison?

---

## 🚀 Practice Exercises

### Beginner

- [ ] Implement linear search for integers
- [ ] Implement linear search for strings
- [ ] Create a struct for a book (title, author, year)
- [ ] Write a recursive countdown function
- [ ] Sort 5 numbers by hand using selection sort

### Intermediate

- [ ] Implement binary search (iterative)
- [ ] Implement bubble sort
- [ ] Create phonebook with structs
- [ ] Write recursive factorial function
- [ ] Trace merge sort step-by-step

### Advanced

- [ ] Implement binary search (recursive)
- [ ] Implement merge sort
- [ ] Compare performance of all sorting algorithms
- [ ] Create complex struct (nested structs)
- [ ] Analyze Big O of custom algorithm

---

## 🎓 Study Tips

### Understanding Big O

1. Don't memorize - understand the pattern
2. Focus on worst case for initial learning
3. Draw graphs to visualize growth
4. Practice identifying O(n), O(n²), O(log n) in code

### Mastering Recursion

1. Start with simple examples (countdown, factorial)
2. Always identify base case first
3. Draw recursion tree on paper
4. Trace execution step-by-step

### Learning Sorting

1. Sort by hand first (4-5 numbers)
2. Watch visualizations online
3. Implement one at a time
4. Compare performance on different inputs

---

_Algorithms are the heart of computer science. Master these fundamentals, and complex problems become manageable!_
