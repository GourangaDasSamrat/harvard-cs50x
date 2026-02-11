# CS50 Lecture 7 - SQL & Databases
## Personal Learning Notes

---

## 🎯 Key Takeaways

- **Databases organize data efficiently** - Better than CSV for large-scale data
- **SQL is a query language** - Different from Python/C, but just as important
- **Relational databases link tables** - Primary and foreign keys connect data
- **CRUD operations** - Create, Read, Update, Delete
- **JOINs combine tables** - Powerful way to relate data
- **Security matters** - Protect against SQL injection attacks

---

## 📁 Flat-File Databases (CSV)

### What is a Flat-File Database?

**Flat-file**: All data in a single table (file)

**CSV** (Comma-Separated Values):
```csv
Timestamp,language,problem
10/25/2023 12:34:56,Python,Hello World
10/25/2023 12:35:12,C,Mario
10/25/2023 12:36:01,Scratch,Scratch
```

**Characteristics**:
- Simple text file
- One table only
- Rows separated by newlines
- Columns separated by commas

---

## 🐍 Reading CSV Files in Python

### Basic CSV Reading

```python
import csv

# Open CSV file
with open("favorites.csv", "r") as file:
    # Create reader
    reader = csv.reader(file)
    
    # Skip header row
    next(reader)
    
    # Iterate over rows
    for row in reader:
        print(row[1])  # Print second column
```

**Problem**: Using `row[1]` is fragile - what if columns move?

### Using DictReader (Better!)

```python
import csv

with open("favorites.csv", "r") as file:
    reader = csv.DictReader(file)
    
    for row in reader:
        print(row["language"])  # Access by name!
```

**Benefits**:
- Access columns by name
- More readable
- Resilient to column reordering

---

## 📊 Counting with Dictionaries

### Manual Counting

```python
import csv

# Open CSV file
with open("favorites.csv", "r") as file:
    reader = csv.DictReader(file)
    
    # Initialize counts
    scratch, c, python = 0, 0, 0
    
    # Count each language
    for row in reader:
        favorite = row["language"]
        if favorite == "Scratch":
            scratch += 1
        elif favorite == "C":
            c += 1
        elif favorite == "Python":
            python += 1
    
    # Print results
    print(f"Scratch: {scratch}")
    print(f"C: {c}")
    print(f"Python: {python}")
```

**Problem**: Must know languages in advance!

### Dynamic Counting with Dictionary

```python
import csv

with open("favorites.csv", "r") as file:
    reader = csv.DictReader(file)
    
    counts = {}
    
    for row in reader:
        favorite = row["language"]
        
        if favorite in counts:
            counts[favorite] += 1
        else:
            counts[favorite] = 1
    
    # Print all counts
    for favorite in counts:
        print(f"{favorite}: {counts[favorite]}")
```

**Better**: Works with any number of languages!

### Using Try-Except

```python
import csv

with open("favorites.csv", "r") as file:
    reader = csv.DictReader(file)
    
    counts = {}
    
    for row in reader:
        favorite = row["language"]
        
        try:
            counts[favorite] += 1
        except KeyError:
            counts[favorite] = 1
    
    for favorite in counts:
        print(f"{favorite}: {counts[favorite]}")
```

**Alternative approach**: Try to increment, create if doesn't exist

---

## 🔢 Sorting Results

### Sort by Key (Alphabetically)

```python
for favorite in sorted(counts):
    print(f"{favorite}: {counts[favorite]}")
```

**Output**:
```
C: 50
Python: 120
Scratch: 30
```

### Sort by Value (Most Popular First)

```python
for favorite in sorted(counts, key=counts.get, reverse=True):
    print(f"{favorite}: {counts[favorite]}")
```

**Parameters**:
- `key=counts.get` - Sort by the value (count)
- `reverse=True` - Highest to lowest

**Output**:
```
Python: 120
C: 50
Scratch: 30
```

---

## 🗄️ Relational Databases

### Why Relational Databases?

**Problems with CSV**:
- ❌ Inefficient for large data
- ❌ No relationships between tables
- ❌ Slow searching
- ❌ Data duplication

**Benefits of Relational Databases**:
- ✅ Efficient storage
- ✅ Fast queries
- ✅ Relationships between tables
- ✅ Data integrity

### Database Tables

**Table**: Collection of rows and columns

```
shows
┌────┬───────────────┬──────┐
│ id │ title         │ year │
├────┼───────────────┼──────┤
│ 1  │ The Office    │ 2005 │
│ 2  │ Breaking Bad  │ 2008 │
│ 3  │ Friends       │ 1994 │
└────┴───────────────┴──────┘
```

### Keys

**Primary Key**: Unique identifier for each row
- Usually named `id`
- Each row has unique value
- Example: `id` in shows table

**Foreign Key**: References primary key in another table
- Links tables together
- Example: `show_id` in ratings references `id` in shows

---

## 📝 SQL Basics

### What is SQL?

**SQL** = Structured Query Language

**Domain-specific language** for working with databases

### CRUD Operations

**C**reate - Add new data
**R**ead - Retrieve data
**U**pdate - Modify existing data
**D**elete - Remove data

---

## 🔍 SELECT - Reading Data

### Basic SELECT

```sql
-- Select all columns, all rows
SELECT * FROM shows;

-- Select specific columns
SELECT title, year FROM shows;

-- Select with limit
SELECT * FROM shows LIMIT 10;
```

**Syntax**:
- `SELECT` - What columns
- `FROM` - Which table
- `;` - End of statement

### Counting and Aggregation

```sql
-- Count all rows
SELECT COUNT(*) FROM favorites;

-- Count distinct values
SELECT COUNT(DISTINCT language) FROM favorites;

-- Other aggregate functions
SELECT AVG(rating) FROM ratings;
SELECT MAX(year) FROM shows;
SELECT MIN(year) FROM shows;
```

**Aggregate functions**:
- `COUNT()` - Count rows
- `AVG()` - Average
- `MAX()` - Maximum
- `MIN()` - Minimum
- `SUM()` - Sum

---

## 🎯 Filtering with WHERE

### Basic Filtering

```sql
-- Single condition
SELECT * FROM favorites WHERE language = 'C';

-- Multiple conditions (AND)
SELECT * FROM favorites 
WHERE language = 'C' AND problem = 'Hello, World';

-- Multiple conditions (OR)
SELECT * FROM favorites
WHERE language = 'C' OR language = 'Python';
```

### Pattern Matching with LIKE

```sql
-- Starts with "Hello"
SELECT * FROM favorites WHERE problem LIKE 'Hello%';

-- Contains "World"
SELECT * FROM favorites WHERE problem LIKE '%World%';

-- Ends with "Mario"
SELECT * FROM favorites WHERE problem LIKE '%Mario';
```

**Wildcards**:
- `%` - Matches any characters
- `_` - Matches single character

---

## 📋 GROUP BY and ORDER BY

### GROUP BY - Grouping Results

```sql
-- Count by language
SELECT language, COUNT(*) FROM favorites
GROUP BY language;
```

**Output**:
```
language | COUNT(*)
---------|----------
C        | 50
Python   | 120
Scratch  | 30
```

### ORDER BY - Sorting Results

```sql
-- Sort alphabetically
SELECT language, COUNT(*) FROM favorites
GROUP BY language
ORDER BY language;

-- Sort by count (ascending)
SELECT language, COUNT(*) FROM favorites
GROUP BY language
ORDER BY COUNT(*);

-- Sort by count (descending)
SELECT language, COUNT(*) FROM favorites
GROUP BY language
ORDER BY COUNT(*) DESC;
```

### Using Aliases

```sql
-- Create alias 'n' for COUNT(*)
SELECT language, COUNT(*) AS n FROM favorites
GROUP BY language
ORDER BY n DESC;
```

**Cleaner and more readable!**

### LIMIT - Limiting Results

```sql
-- Top 3 most popular
SELECT language, COUNT(*) AS n FROM favorites
GROUP BY language
ORDER BY n DESC
LIMIT 3;
```

---

## ➕ INSERT - Adding Data

### Basic INSERT

```sql
-- Insert into specific columns
INSERT INTO favorites (language, problem)
VALUES ('SQL', 'Fiftyville');

-- Insert multiple rows
INSERT INTO favorites (language, problem)
VALUES 
    ('SQL', 'Fiftyville'),
    ('Python', 'DNA'),
    ('C', 'Caesar');
```

**Syntax**:
- Specify table name
- List columns in parentheses
- Provide values for each column

---

## 🗑️ DELETE - Removing Data

### Basic DELETE

```sql
-- Delete specific rows
DELETE FROM favorites WHERE language = 'Scratch';

-- Delete with multiple conditions
DELETE FROM favorites 
WHERE language = 'C' AND problem = 'Mario';

-- Delete all rows (dangerous!)
DELETE FROM favorites;
```

**⚠️ WARNING**: No `WHERE` clause deletes ALL rows!

### Safe Deletion

```sql
-- Delete rows with NULL timestamp
DELETE FROM favorites WHERE Timestamp IS NULL;
```

---

## 🔄 UPDATE - Modifying Data

### Basic UPDATE

```sql
-- Update specific rows
UPDATE favorites
SET language = 'SQL', problem = 'Fiftyville'
WHERE language = 'C';

-- Update all rows (dangerous!)
UPDATE favorites
SET language = 'Python';
```

**⚠️ WARNING**: Without `WHERE`, updates ALL rows!

### Safe Update

```sql
-- Update specific record
UPDATE favorites
SET language = 'Python'
WHERE id = 42;
```

---

## 🔗 Relationships and JOINs

### Database Relationships

**One-to-One**: One record relates to one record
```
shows ←→ ratings
(One show has one rating)
```

**One-to-Many**: One record relates to many records
```
shows ←→ genres
(One show has many genres)
```

**Many-to-Many**: Many records relate to many records
```
shows ←→ people
(Many shows have many people, people appear in many shows)
```

### JOIN Tables

**JOIN**: Combine columns from multiple tables

```sql
-- Basic JOIN
SELECT * FROM shows
JOIN ratings ON shows.id = ratings.show_id
WHERE rating >= 6.0
LIMIT 10;
```

**How it works**:
1. Take `shows` table
2. For each row, find matching row in `ratings`
3. Match on `shows.id = ratings.show_id`
4. Combine into wider table
5. Filter and limit

### Visualizing JOINs

**Before JOIN**:
```
shows                    ratings
┌────┬─────────┐        ┌─────────┬────────┐
│ id │ title   │        │ show_id │ rating │
├────┼─────────┤        ├─────────┼────────┤
│ 1  │ Office  │        │ 1       │ 8.5    │
│ 2  │ Friends │        │ 2       │ 7.2    │
└────┴─────────┘        └─────────┴────────┘
```

**After JOIN**:
```
┌────┬─────────┬─────────┬────────┐
│ id │ title   │ show_id │ rating │
├────┼─────────┼─────────┼────────┤
│ 1  │ Office  │ 1       │ 8.5    │
│ 2  │ Friends │ 2       │ 7.2    │
└────┴─────────┴─────────┴────────┘
```

### Multiple JOINs

```sql
-- Find all shows Steve Carell starred in
SELECT title FROM shows
JOIN stars ON shows.id = stars.show_id
JOIN people ON stars.person_id = people.id
WHERE name = 'Steve Carell';
```

**Flow**:
1. Join `shows` with `stars` (via show_id)
2. Join result with `people` (via person_id)
3. Filter by name

---

## 🔍 Nested Queries (Subqueries)

### Basic Subquery

```sql
-- Find titles of highly-rated shows
SELECT title FROM shows
WHERE id IN (
    SELECT show_id FROM ratings
    WHERE rating >= 6.0
    LIMIT 10
);
```

**How it works**:
1. Inner query runs first: Get show_ids with rating >= 6.0
2. Outer query uses those ids: Get titles

### Multiple Levels

```sql
-- Find shows Steve Carell starred in
SELECT title FROM shows WHERE id IN
    (SELECT show_id FROM stars WHERE person_id =
        (SELECT id FROM people WHERE name = 'Steve Carell')
    );
```

**Three levels**:
1. Innermost: Get Steve Carell's id
2. Middle: Get show_ids he starred in
3. Outer: Get show titles

**Alternative using JOIN** (often clearer):
```sql
SELECT title FROM shows
JOIN stars ON shows.id = stars.show_id
JOIN people ON stars.person_id = people.id
WHERE name = 'Steve Carell';
```

---

## 📊 Creating Tables

### CREATE TABLE Syntax

```sql
CREATE TABLE favorites (
    id INTEGER PRIMARY KEY,
    language TEXT NOT NULL,
    problem TEXT NOT NULL,
    Timestamp TEXT
);
```

**Column definitions**:
- `id INTEGER PRIMARY KEY` - Unique identifier
- `TEXT NOT NULL` - String, required
- `TEXT` - String, optional

### Data Types in SQLite

| Type | Description | Example |
|------|-------------|---------|
| `INTEGER` | Whole numbers | 42, -10 |
| `REAL` | Floating point | 3.14, -0.5 |
| `TEXT` | String | "Hello" |
| `BLOB` | Binary data | Images, files |
| `NUMERIC` | Flexible number | Dates, times |

### Constraints

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL,
    email TEXT UNIQUE
);
```

**Common constraints**:
- `PRIMARY KEY` - Unique identifier
- `NOT NULL` - Must have value
- `UNIQUE` - No duplicates allowed
- `DEFAULT value` - Default if not provided

---

## ⚡ Indexes for Speed

### Why Indexes?

**Without index**: Must scan entire table (slow!)
**With index**: Direct lookup using data structure (fast!)

### Creating Indexes

```sql
-- Create index on title
CREATE INDEX title_index ON shows (title);

-- Create index on name
CREATE INDEX name_index ON people (name);

-- Multiple indexes
CREATE INDEX person_index ON stars (person_id);
```

### How Indexes Work

**B-Tree data structure** (like binary tree, but each node can have 3+ children)

```
        M
       / \
      /   \
     D     S
    /|\   /|\
   A C E Q R T
```

**Trade-off**:
- ✅ Much faster searches
- ❌ Uses more storage space
- ❌ Slower inserts/updates

### When to Use Indexes

**Create indexes on columns that are**:
- Frequently searched
- Used in WHERE clauses
- Used in JOIN conditions

**Don't index**:
- Every column (waste of space)
- Rarely queried columns
- Small tables

---

## 🐍 Using SQL in Python

### CS50 SQL Library

```python
from cs50 import SQL

# Open database
db = SQL("sqlite:///favorites.db")

# Execute query
rows = db.execute("SELECT * FROM favorites WHERE language = ?", "Python")

# Iterate results
for row in rows:
    print(row["problem"])
```

**Key points**:
- `db.execute()` runs SQL
- Returns list of dictionaries
- Use `?` placeholders for values

### Parameterized Queries

```python
# Get user input
favorite = input("Favorite: ")

# Safe query with placeholder
rows = db.execute(
    "SELECT COUNT(*) AS n FROM favorites WHERE problem = ?",
    favorite
)

# Access result
count = rows[0]["n"]
print(count)
```

**Why `?` placeholders?**
- Prevents SQL injection
- Automatically escapes special characters
- Required for security!

### Complete Example

```python
from cs50 import SQL

# Open database
db = SQL("sqlite:///favorites.db")

# Get user input
language = input("Language: ")

# Query database
rows = db.execute(
    "SELECT problem, COUNT(*) AS n FROM favorites WHERE language = ? GROUP BY problem ORDER BY n DESC",
    language
)

# Display results
for row in rows:
    print(f"{row['problem']}: {row['n']}")
```

---

## 🔐 Security Concerns

### SQL Injection Attacks

**Danger**: User input directly in SQL

```python
# DANGEROUS! ❌
username = input("Username: ")
password = input("Password: ")

rows = db.execute(
    f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
)
```

**Attack**: User enters `admin' --`
```sql
-- Becomes:
SELECT * FROM users WHERE username = 'admin' -- ' AND password = ''
-- Everything after -- is comment!
-- Logs in without password!
```

### Prevention: Parameterized Queries

```python
# SAFE! ✅
username = input("Username: ")
password = input("Password: ")

rows = db.execute(
    "SELECT * FROM users WHERE username = ? AND password = ?",
    username, password
)
```

**CS50 Library sanitizes input** - removes malicious characters!

### Best Practices

1. **Always use placeholders** (`?`)
2. **Never use f-strings** for SQL
3. **Never concatenate** user input
4. **Validate input** before using
5. **Use CS50 library** or prepared statements

---

## 🏁 Race Conditions

### What Are Race Conditions?

**Problem**: Multiple users accessing database simultaneously

**Example scenario**:
1. User A reads balance: $100
2. User B reads balance: $100
3. User A withdraws $50, updates to $50
4. User B withdraws $30, updates to $70 (Wrong!)

Should be $20, but both read before either updated!

### SQL Transactions

```sql
-- Start transaction
BEGIN TRANSACTION;

-- Multiple operations
UPDATE accounts SET balance = balance - 50 WHERE id = 1;
UPDATE accounts SET balance = balance + 50 WHERE id = 2;

-- Commit if successful
COMMIT;

-- Or rollback if error
ROLLBACK;
```

**Benefits**:
- All operations succeed or all fail
- No partial updates
- Prevents race conditions

---

## 🗂️ SQLite Commands

### Useful SQLite Commands

```bash
# Start SQLite
sqlite3 favorites.db

# Set mode to CSV
.mode csv

# Import CSV file
.import favorites.csv favorites

# Show table structure
.schema
.schema favorites

# Show tables
.tables

# Enable timer
.timer on

# Exit
.quit
```

---

## 📊 Real-World Example: IMDb

### Database Schema

```
shows                ratings              people
┌────┬───────┬────┐  ┌─────────┬────┐    ┌────┬────────┐
│ id │ title │ yr │  │ show_id │ rt │    │ id │ name   │
└────┴───────┴────┘  └─────────┴────┘    └────┴────────┘
                            ↓                     ↓
                            └─────────┬───────────┘
                                      │
                                   stars
                            ┌─────────┬───────────┐
                            │ show_id │ person_id │
                            └─────────┴───────────┘
```

### Complex Query Example

```sql
-- Find comedies rated 8.0+
SELECT title, rating FROM shows
JOIN ratings ON shows.id = ratings.show_id
JOIN genres ON shows.id = genres.show_id
WHERE genre = 'Comedy' AND rating >= 8.0
ORDER BY rating DESC;
```

---

## 💡 SQL Best Practices

### Writing Clean SQL

1. **Use UPPERCASE for keywords**
```sql
SELECT title FROM shows;  -- Good
select title from shows;  -- Works, but less clear
```

2. **Indent for readability**
```sql
SELECT title, year
FROM shows
WHERE year > 2000
ORDER BY year DESC;
```

3. **Use meaningful aliases**
```sql
SELECT COUNT(*) AS total_shows
FROM shows;
```

4. **Comment complex queries**
```sql
-- Find shows with more than 100 episodes
SELECT title, COUNT(*) AS episodes
FROM shows
JOIN episodes ON shows.id = episodes.show_id
GROUP BY title
HAVING episodes > 100;
```

---

## ⚠️ Common Mistakes

### 1. Forgetting WHERE in UPDATE/DELETE

```sql
-- WRONG - Deletes everything! ❌
DELETE FROM users;

-- CORRECT - Deletes specific records ✅
DELETE FROM users WHERE id = 42;
```

### 2. SQL Injection

```python
# WRONG - Vulnerable to injection! ❌
query = f"SELECT * FROM users WHERE name = '{name}'"

# CORRECT - Use placeholders ✅
query = db.execute("SELECT * FROM users WHERE name = ?", name)
```

### 3. Not Using Indexes

```sql
-- Slow without index
SELECT * FROM shows WHERE title = 'The Office';

-- Create index for speed
CREATE INDEX title_index ON shows (title);
```

### 4. Confusing = and ==

```sql
-- SQL uses single = for comparison
WHERE language = 'Python'

-- NOT ==
WHERE language == 'Python'  -- ERROR!
```

---

## ✅ Self-Check Questions

1. Can I explain the difference between flat-file and relational databases?
2. Do I know the basic SQL commands (SELECT, INSERT, UPDATE, DELETE)?
3. Can I use WHERE to filter results?
4. Do I understand JOINs and when to use them?
5. Can I write parameterized queries in Python?
6. Do I know why SQL injection is dangerous?
7. Can I explain primary and foreign keys?
8. Do I understand when to use indexes?

---

## 🚀 Practice Exercises

### Beginner
- [ ] Read a CSV file in Python and count values
- [ ] Create a SQLite database and insert data
- [ ] Write SELECT queries with WHERE
- [ ] Use GROUP BY to aggregate data
- [ ] Create a simple index

### Intermediate
- [ ] JOIN two tables
- [ ] Use nested queries (subqueries)
- [ ] Write parameterized queries in Python
- [ ] Create a database schema with foreign keys
- [ ] Use ORDER BY and LIMIT

### Advanced
- [ ] Design a multi-table database
- [ ] Write complex queries with multiple JOINs
- [ ] Optimize queries with indexes
- [ ] Handle SQL injection attempts
- [ ] Use transactions for data integrity

---

## 📌 Important Reminders

1. **Always use placeholders** (`?`) in Python SQL
2. **Primary keys** uniquely identify rows
3. **Foreign keys** link tables together
4. **JOINs** combine tables on matching keys
5. **Indexes** speed up queries but use space
6. **WHERE clause** filters results
7. **GROUP BY** aggregates data
8. **ORDER BY** sorts results
9. **SQL keywords** are case-insensitive (but use CAPS)
10. **Backup before UPDATE/DELETE!**

---

*SQL is the language of data. Master it, and you can work with databases that power the entire internet!*