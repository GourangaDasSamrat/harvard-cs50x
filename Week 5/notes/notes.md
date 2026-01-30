# CS50 Lecture 5 - Data Structures
## Personal Learning Notes

---

## 🎯 Key Takeaways

- **Data structures organize data in memory** - Different structures for different needs
- **Trade-offs are everywhere** - Speed vs memory, simplicity vs functionality
- **Abstract data types** - Conceptual structures (queues, stacks)
- **Concrete implementations** - How to build them in code
- **The `->` operator** - Access members of a struct via pointer
- **Dynamic memory** - Grow and shrink structures as needed

---

## 📚 Abstract Data Types

### What Are Abstract Data Types?

**Abstract Data Types (ADTs)**: Conceptual data structures defined by behavior, not implementation

**Think of them as**: The "what" (what they do) not the "how" (how they're built)

---

## 📥 Queues (FIFO)

### Concept

**FIFO** = First In, First Out

**Real-world analogy**: Line at an amusement park
- First person in line → First to ride
- Last person in line → Last to ride

### Queue Operations

**Enqueue**: Add to the back of the queue
**Dequeue**: Remove from the front of the queue

**Visual**:
```
Enqueue 1 → [1]
Enqueue 2 → [1, 2]
Enqueue 3 → [1, 2, 3]
Dequeue   → [2, 3]    (removed 1)
Dequeue   → [3]       (removed 2)
```

### Queue Implementation

```c
const int CAPACITY = 50;

typedef struct
{
    person people[CAPACITY];
    int size;
}
queue;
```

**Structure**:
- `people[]` - Array to hold items
- `size` - Current number of items
- `CAPACITY` - Maximum capacity

---

## 📤 Stacks (LIFO)

### Concept

**LIFO** = Last In, First Out

**Real-world analogy**: Stack of dining hall trays
- Last tray placed → First to be taken
- First tray placed → Last to be taken

### Stack Operations

**Push**: Add to the top of the stack
**Pop**: Remove from the top of the stack

**Visual**:
```
Push 1 → [1]
Push 2 → [2, 1]
Push 3 → [3, 2, 1]
Pop    → [2, 1]    (removed 3)
Pop    → [1]       (removed 2)
```

### Stack Implementation

```c
const int CAPACITY = 50;

typedef struct
{
    person people[CAPACITY];
    int size;
}
stack;
```

**Same structure as queue** - difference is in how we use it!

---

## 📦 Arrays (Review & Dynamic Arrays)

### Fixed-Size Arrays (Week 2)

**Problem**: Size is predetermined and can't change

```c
#include <stdio.h>

int main(void)
{
    int list[3];  // Fixed size: 3
    
    list[0] = 1;
    list[1] = 2;
    list[2] = 3;
    
    // What if we need to add a 4th item? Can't!
}
```

**Memory visualization**:
```
┌───┬───┬───┐
│ 1 │ 2 │ 3 │
└───┴───┴───┘
```

### Dynamic Arrays with malloc()

**Solution**: Allocate new memory, copy old data, add new item

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    // List of size 3
    int *list = malloc(3 * sizeof(int));
    if (list == NULL) return 1;
    
    list[0] = 1;
    list[1] = 2;
    list[2] = 3;
    
    // Need size 4? Create new array
    int *tmp = malloc(4 * sizeof(int));
    if (tmp == NULL)
    {
        free(list);
        return 1;
    }
    
    // Copy old to new
    for (int i = 0; i < 3; i++)
    {
        tmp[i] = list[i];
    }
    
    // Add new value
    tmp[3] = 4;
    
    // Free old array
    free(list);
    
    // Point to new array
    list = tmp;
    
    // Print list
    for (int i = 0; i < 4; i++)
    {
        printf("%i\n", list[i]);
    }
    
    free(list);
    return 0;
}
```

**Process visualization**:

1. **Original array**:
```
list → [1, 2, 3]
```

2. **Create larger array**:
```
list → [1, 2, 3]
tmp  → [?, ?, ?, ?]  (garbage values)
```

3. **Copy values**:
```
list → [1, 2, 3]
tmp  → [1, 2, 3, ?]
```

4. **Add new value**:
```
list → [1, 2, 3]
tmp  → [1, 2, 3, 4]
```

5. **Free old, point to new**:
```
list → [1, 2, 3, 4]  (now points where tmp pointed)
```

### Using realloc()

**Easier way**: Let `realloc()` handle the copying!

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    // List of size 3
    int *list = malloc(3 * sizeof(int));
    if (list == NULL) return 1;
    
    list[0] = 1;
    list[1] = 2;
    list[2] = 3;
    
    // Resize to size 4
    int *tmp = realloc(list, 4 * sizeof(int));
    if (tmp == NULL)
    {
        free(list);
        return 1;
    }
    list = tmp;
    
    // Add new value
    list[3] = 4;
    
    // Print and free
    for (int i = 0; i < 4; i++)
    {
        printf("%i\n", list[i]);
    }
    
    free(list);
    return 0;
}
```

**What `realloc()` does**:
- Tries to expand existing memory block
- If can't, allocates new block and copies data
- Returns pointer to memory (old or new location)

**Problem with dynamic arrays**: Still need to copy everything when resizing - `O(n)` operation!

---

## 🔗 Linked Lists

### The Core Idea

**Problem**: Arrays require contiguous memory and copying to resize

**Solution**: Link separate chunks of memory together!

### Linked List Structure

**Node**: Basic building block
```c
typedef struct node
{
    int number;           // Data
    struct node *next;    // Pointer to next node
}
node;
```

**Key components**:
- **Data field** (`number`) - The actual value
- **Next pointer** (`next`) - Address of next node
- **NULL** - Indicates end of list

### Visual Representation

**Conceptual**:
```
[1] → [2] → [3] → NULL
```

**In memory**:
```
     ┌─────┬──────┐      ┌─────┬──────┐      ┌─────┬──────┐
     │  1  │  •───┼─────→│  2  │  •───┼─────→│  3  │ NULL │
     └─────┴──────┘      └─────┴──────┘      └─────┴──────┘
```

**With addresses**:
```
Address: 0x123          0x456          0x789
        ┌─────┬──────┐ ┌─────┬──────┐ ┌─────┬──────┐
        │  1  │ 0x456├→│  2  │ 0x789├→│  3  │ NULL │
        └─────┴──────┘ └─────┴──────┘ └─────┴──────┘
         ↑
         │
       head
```

### The `->` Operator

**New operator!** Used to access struct members through a pointer

**Syntax**: `pointer->member`

**Equivalent to**: `(*pointer).member`

**Examples**:
```c
node *n = malloc(sizeof(node));

// Using ->
n->number = 5;
n->next = NULL;

// Equivalent using * and .
(*n).number = 5;
(*n).next = NULL;
```

**Why `->` is better**: Cleaner, more readable!

### Building a Linked List (Prepending)

**Adding to the front** - Fast! `O(1)`

```c
#include <cs50.h>
#include <stdio.h>
#include <stdlib.h>

typedef struct node
{
    int number;
    struct node *next;
}
node;

int main(void)
{
    // Initially empty list
    node *list = NULL;
    
    // Add 3 numbers
    for (int i = 0; i < 3; i++)
    {
        // Allocate new node
        node *n = malloc(sizeof(node));
        if (n == NULL) return 1;
        
        // Set values
        n->number = get_int("Number: ");
        n->next = NULL;
        
        // Prepend to list
        n->next = list;  // Point to current head
        list = n;        // New node becomes head
    }
    
    return 0;
}
```

**Step-by-step visualization**:

**Step 1**: Start with empty list
```
list = NULL
```

**Step 2**: Add first node (value: 1)
```
n → [1|NULL]

n->next = list  (NULL)
list = n

Result:
list → [1|NULL]
```

**Step 3**: Add second node (value: 2)
```
       [1|NULL]
        ↑
n → [2|?]

n->next = list
list = n

Result:
list → [2] → [1|NULL]
```

**Step 4**: Add third node (value: 3)
```
       [2] → [1|NULL]
        ↑
n → [3|?]

n->next = list
list = n

Result:
list → [3] → [2] → [1|NULL]
```

**Note**: Numbers are in reverse order! (Last added appears first)

### Printing a Linked List

```c
// Print all nodes
node *ptr = list;
while (ptr != NULL)
{
    printf("%i\n", ptr->number);
    ptr = ptr->next;  // Move to next node
}
```

**How it works**:
1. `ptr` starts at head
2. Print current node's value
3. Move `ptr` to next node
4. Repeat until `ptr` is `NULL`

### Appending to a Linked List

**Adding to the end** - Slower! `O(n)` because we must traverse entire list

```c
// Allocate new node
node *n = malloc(sizeof(node));
if (n == NULL) return 1;
n->number = get_int("Number: ");
n->next = NULL;

// If list is empty
if (list == NULL)
{
    list = n;
}
else
{
    // Find end of list
    for (node *ptr = list; ptr != NULL; ptr = ptr->next)
    {
        if (ptr->next == NULL)  // At the end!
        {
            ptr->next = n;  // Append new node
            break;
        }
    }
}
```

### Inserting in Sorted Order

**Maintain sorted list** - `O(n)` for insertion

```c
// Allocate new node
node *n = malloc(sizeof(node));
if (n == NULL) return 1;
n->number = get_int("Number: ");
n->next = NULL;

// If list is empty
if (list == NULL)
{
    list = n;
}
// If belongs at beginning
else if (n->number < list->number)
{
    n->next = list;
    list = n;
}
// Belongs somewhere in middle/end
else
{
    for (node *ptr = list; ptr != NULL; ptr = ptr->next)
    {
        // At end of list
        if (ptr->next == NULL)
        {
            ptr->next = n;
            break;
        }
        
        // Found insertion point
        if (n->number < ptr->next->number)
        {
            n->next = ptr->next;  // Point to next
            ptr->next = n;         // Insert here
            break;
        }
    }
}
```

### Freeing a Linked List

**CRITICAL**: Must free each node!

**WRONG** ❌:
```c
node *ptr = list;
while (ptr != NULL)
{
    free(ptr);
    ptr = ptr->next;  // ERROR! Can't access freed memory!
}
```

**CORRECT** ✅:
```c
node *ptr = list;
while (ptr != NULL)
{
    node *next = ptr->next;  // Save next pointer
    free(ptr);                // Free current node
    ptr = next;               // Move to saved next
}
```

**Why this works**: We save the next pointer BEFORE freeing!

### Linked List Trade-offs

**Advantages**:
- ✅ Dynamic size - grows/shrinks as needed
- ✅ No wasted memory (only allocate what's needed)
- ✅ Fast insertion at front - `O(1)`
- ✅ No copying required when adding elements

**Disadvantages**:
- ❌ Extra memory per element (for pointer)
- ❌ No random access (must traverse)
- ❌ No binary search (not contiguous)
- ❌ Insertion at end is `O(n)`
- ❌ Search is `O(n)`

---

## 🌲 Binary Search Trees (BST)

### The Goal

**Combine benefits of**:
- Arrays: Fast searching (binary search)
- Linked lists: Dynamic sizing

**Solution**: Binary Search Tree!

### Tree Structure

**Node**:
```c
typedef struct node
{
    int number;
    struct node *left;   // Left child
    struct node *right;  // Right child
}
node;
```

### Tree Organization

**Rule**: 
- Left child < Parent
- Right child > Parent

**Example tree**:
```
       4
      / \
     2   6
    / \ / \
   1  3 5  7
```

**In this tree**:
- 4 is the root
- Everything left of 4 is < 4
- Everything right of 4 is > 4
- Same rule applies recursively to subtrees

### Building from Sorted Array

**Given**: [1, 2, 3, 4, 5, 6, 7]

**Step 1**: Middle becomes root
```
    4
```

**Step 2**: Left half (< 4) goes left
```
    4
   /
  2
 / \
1   3
```

**Step 3**: Right half (> 4) goes right
```
       4
      / \
     2   6
    / \ / \
   1  3 5  7
```

### Searching a BST

**Recursive algorithm**:

```c
bool search(node *tree, int number)
{
    // Base case: empty tree
    if (tree == NULL)
    {
        return false;
    }
    // Search left subtree
    else if (number < tree->number)
    {
        return search(tree->left, number);
    }
    // Search right subtree
    else if (number > tree->number)
    {
        return search(tree->right, number);
    }
    // Found it!
    else
    {
        return true;
    }
}
```

**Example search for 6**:
```
Start at 4: 6 > 4, go right
At 6: 6 == 6, found! Return true
```

**Example search for 5**:
```
Start at 4: 5 > 4, go right
At 6: 5 < 6, go left
At 5: 5 == 5, found! Return true
```

### BST Performance

**Balanced tree**:
- Search: `O(log n)` ✅
- Insert: `O(log n)` ✅
- Like binary search!

**Unbalanced tree** (worst case):
```
1
 \
  2
   \
    3
     \
      4
```
- Essentially a linked list!
- Search: `O(n)` ❌
- Insert: `O(n)` ❌

**Key insight**: Tree must stay balanced for good performance!

---

## #️⃣ Hash Tables

### The Ultimate Goal

**O(1)** - Constant time access!

**Holy grail**: Instant lookup, regardless of data size

### What is Hashing?

**Hashing**: Convert input to array index

**Hash function**: Algorithm that takes input → returns index

**Example**:
```
"apple"  → hash → 0
"banana" → hash → 1
"cherry" → hash → 2
```

### Simple Hash Function

**Alphabetic hashing** - First letter determines bucket

```c
#include <ctype.h>

unsigned int hash(const char *word)
{
    return toupper(word[0]) - 'A';
}
```

**How it works**:
```
"apple"  → 'A' → 0
"banana" → 'B' → 1
"cherry" → 'C' → 2
...
"zebra"  → 'Z' → 25
```

### Hash Table Structure

**Array of linked lists**:

```
Index  Bucket
  0  →  [apple] → [avocado] → NULL
  1  →  [banana] → [blueberry] → NULL
  2  →  [cherry] → NULL
  3  →  NULL
  ...
 25  →  [zebra] → NULL
```

**Each bucket is a linked list** to handle collisions!

### Collisions

**Collision**: Multiple items hash to same index

**Example**:
```
"apple"   → hash → 0
"avocado" → hash → 0  (collision!)
```

**Solution**: Chain them in a linked list!

```
  0  →  [apple] → [avocado] → NULL
```

### Visual Representation

```
┌─────┬──────┐
│  0  │  •───┼──→ [apple] → [avocado] → NULL
├─────┼──────┤
│  1  │  •───┼──→ [banana] → NULL
├─────┼──────┤
│  2  │  •───┼──→ [cherry] → [carrot] → [cucumber] → NULL
├─────┼──────┤
│  3  │ NULL │
├─────┼──────┤
│ ... │ ...  │
├─────┼──────┤
│ 25  │  •───┼──→ [zebra] → NULL
└─────┴──────┘
```

### Better Hash Functions

**Problem**: First-letter hash causes many collisions

**Better approach**: Use more characters

```c
unsigned int hash(const char *word)
{
    unsigned int hash_value = 0;
    for (int i = 0; word[i] != '\0'; i++)
    {
        hash_value += toupper(word[i]);
    }
    return hash_value % HASH_TABLE_SIZE;
}
```

**Even better**: Consider position and weighting
```c
// Use each character's position
hash_value = (hash_value * 31 + word[i]) % SIZE;
```

### Hash Table Performance

**Best case**: `O(1)`
- Perfect hash function
- No collisions
- Direct array access

**Worst case**: `O(n)`
- All items hash to same bucket
- Essentially a linked list
- Must traverse entire chain

**Average case**: `O(n/k)` where k = number of buckets
- Approaches `O(1)` as k increases
- Trade-off: More buckets = more memory

### Trade-offs

**More buckets**:
- ✅ Fewer collisions
- ✅ Faster lookup
- ❌ More memory used

**Fewer buckets**:
- ✅ Less memory used
- ❌ More collisions
- ❌ Slower lookup

---

## 🔤 Tries (Prefix Trees)

### Concept

**Trie** (pronounced "try"): Tree of arrays

**Special property**: `O(1)` lookup time!

**How?** Time depends on key length, not dataset size!

### Structure

**Each node** contains:
- Array of 26 pointers (one per letter)
- Boolean flag (is this a complete word?)

```c
typedef struct node
{
    bool is_word;
    struct node *children[26];
}
node;
```

### Storing Words

**Example**: Store "CAT" and "CATS"

```
Root
 └─ C (index 2)
     └─ A (index 0)
         └─ T (index 19) ✓ (is_word = true)
             └─ S (index 18) ✓ (is_word = true)
```

**Visual representation**:
```
         Root
          │
    ┌─────┼─────┐
    0     2     25
   (A)   (C)   (Z)
          │
    ┌─────┼─────┐
    0     1     25
   (A)   (B)   (Z)
    │
    └──> T ✓ (word: "CAT")
          │
          └──> S ✓ (word: "CATS")
```

### Searching in a Trie

**To find "CAT"**:

1. Start at root
2. Go to C's child (index 2)
3. Go to A's child (index 0)
4. Go to T's child (index 19)
5. Check `is_word` flag → TRUE!

**Time**: O(k) where k = length of word

**For any size dataset**: Same time!

### Detailed Example: "TOAD" and "TOM"

**Storing "TOAD"**:

```
Root → T → O → A → D ✓
```

**Then storing "TOM"**:

```
Root → T → O → A → D ✓
            ↓
            M ✓
```

**Full visualization**:
```
                 Root
                  │
              ┌───T───┐
              │       │
          ┌───O───┐   ...
          │       │
      ┌───A───┐   M ✓ ("TOM")
      │       │
      D ✓     ...
  ("TOAD")
```

### Trie Performance

**Search time**: `O(1)` (constant!)
- More precisely: `O(k)` where k = key length
- Independent of number of words stored!

**Memory cost**: HUGE! ❌

**Example**: Storing just "TOAD" (4 letters):
- 4 nodes × 26 pointers each = 104 pointers!
- Most are NULL (wasted space)

### Trade-offs

**Advantages**:
- ✅ Constant-time lookup
- ✅ No collisions
- ✅ Great for autocomplete
- ✅ Prefix searching

**Disadvantages**:
- ❌ Huge memory usage
- ❌ 26 pointers per node (mostly NULL)
- ❌ Complex to implement
- ❌ Not cache-friendly

---

## 📊 Data Structures Comparison

### Summary Table

| Structure | Search | Insert | Delete | Memory | Notes |
|-----------|--------|--------|--------|--------|-------|
| **Array** | O(n) | O(n) | O(n) | Efficient | Binary search O(log n) if sorted |
| **Sorted Array** | O(log n) | O(n) | O(n) | Efficient | Insertion/deletion expensive |
| **Linked List** | O(n) | O(1)* | O(1)* | + pointers | *At front; O(n) at end |
| **Hash Table** | O(1)** | O(1)** | O(1)** | High | **Average case |
| **Binary Search Tree** | O(log n)*** | O(log n)*** | O(log n)*** | Moderate | ***If balanced |
| **Trie** | O(1) | O(1) | O(1) | Very high | Time = key length |

### When to Use Each

**Array**:
- ✅ Fixed size known
- ✅ Random access needed
- ✅ Small dataset

**Linked List**:
- ✅ Frequent insertion/deletion at front
- ✅ Size unknown
- ✅ No random access needed

**Hash Table**:
- ✅ Need fast lookup
- ✅ Large dataset
- ✅ Have memory to spare

**Binary Search Tree**:
- ✅ Need sorted order
- ✅ Frequent insertion/deletion
- ✅ Moderate dataset

**Trie**:
- ✅ String/prefix operations
- ✅ Autocomplete
- ✅ Memory not a constraint

---

## 💡 Key Concepts

### The `->` Operator

**Essential for data structures!**

```c
node *n = malloc(sizeof(node));

// These are equivalent:
n->number = 5;
(*n).number = 5;

// Always use -> for clarity!
```

### Memory Management Rules

1. **malloc()** for every node
2. **Check for NULL** after malloc
3. **Free in correct order** (save next pointer first!)
4. **No memory leaks** - free everything allocated

### Big O Complexity

**Remember**:
- O(1) = Constant (best!)
- O(log n) = Logarithmic (great!)
- O(n) = Linear (okay)
- O(n log n) = Linearithmic (acceptable)
- O(n²) = Quadratic (slow!)

---

## ⚠️ Common Mistakes

### 1. Freeing Before Accessing Next

```c
// WRONG ❌
while (ptr != NULL)
{
    free(ptr);
    ptr = ptr->next;  // ERROR! ptr is freed!
}

// CORRECT ✅
while (ptr != NULL)
{
    node *next = ptr->next;
    free(ptr);
    ptr = next;
}
```

### 2. Losing Track of Head

```c
// WRONG ❌
for (node *ptr = list; ptr != NULL; ptr = ptr->next)
{
    // After this loop, list is lost if not careful!
}

// CORRECT ✅
node *ptr = list;  // Use temporary pointer
while (ptr != NULL)
{
    // list is preserved!
}
```

### 3. Forgetting to Check NULL

```c
// WRONG ❌
node *n = malloc(sizeof(node));
n->number = 5;  // CRASH if malloc failed!

// CORRECT ✅
node *n = malloc(sizeof(node));
if (n == NULL) return 1;
n->number = 5;
```

### 4. Incorrect Hash Function

```c
// BAD - many collisions ❌
unsigned int hash(const char *word)
{
    return word[0] - 'A';  // Only uses first letter
}

// BETTER - fewer collisions ✅
unsigned int hash(const char *word)
{
    unsigned int value = 0;
    for (int i = 0; word[i]; i++)
    {
        value = (value * 31 + word[i]) % SIZE;
    }
    return value;
}
```

---

## ✅ Self-Check Questions

1. Can I explain the difference between FIFO and LIFO?
2. Do I understand what the `->` operator does?
3. Can I implement a linked list from scratch?
4. Do I know how to properly free a linked list?
5. Can I explain why BST search is O(log n)?
6. Do I understand how hash tables achieve O(1)?
7. Can I explain the trade-off between tries and hash tables?
8. Do I know when to use each data structure?

---

## 🚀 Practice Exercises

### Beginner
- [ ] Implement a stack using an array
- [ ] Implement a queue using an array
- [ ] Create a simple linked list
- [ ] Write a function to print a linked list
- [ ] Free a linked list properly

### Intermediate
- [ ] Implement sorted insertion in linked list
- [ ] Create a simple hash table
- [ ] Write a hash function
- [ ] Implement BST insertion
- [ ] Implement BST search

### Advanced
- [ ] Implement doubly-linked list
- [ ] Build a balanced BST from sorted array
- [ ] Create a spell-checker with hash table
- [ ] Implement autocomplete with trie
- [ ] Compare performance of different structures

---

## 🎓 Study Tips

### Understanding Pointers in Structures
1. **Draw diagrams** - Box and arrow diagrams are essential
2. **Trace code by hand** - Follow each pointer step-by-step
3. **Use `->` consistently** - Don't mix with `(*ptr).`
4. **Check for NULL** - Always, before dereferencing

### Mastering Linked Lists
1. **Start simple** - Build basic list first
2. **Draw each step** - Visualize pointer changes
3. **Practice freeing** - Memory management is crucial
4. **Edge cases** - Empty list, single node, etc.

### Choosing Data Structures
1. **Know trade-offs** - Time vs space
2. **Consider operations** - What's most common?
3. **Think about size** - How much data?
4. **Memory matters** - Can you afford tries?

---

## 📌 Important Reminders

1. **`->` accesses struct members** through pointers
2. **Always check malloc()** for NULL
3. **Save next pointer** before freeing nodes
4. **Hash tables trade memory for speed**
5. **Tries offer O(1)** but use lots of memory
6. **BST must be balanced** for good performance
7. **Abstract vs concrete** - Concept vs implementation
8. **Every malloc needs a free** - No exceptions!
9. **Draw diagrams** - Seriously, always draw them
10. **Trade-offs everywhere** - No perfect structure

---

*Data structures are the foundation of efficient programming. Understanding when and how to use each one is a crucial skill that will serve you throughout your career!*