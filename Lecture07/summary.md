# 🗄️ CS50 Lecture 7: SQL

> **Course**: CS50 - Harvard University's Introduction to Computer Science  
> **Lecture**: Week 7 - SQL (Structured Query Language)  
> **Focus**: Databases, data relationships, and efficient data querying

---

## 🎯 Learning Objectives

By the end of this lecture, you will understand:
- Flat-file databases vs. relational databases
- SQL syntax for creating, reading, updating, and deleting data
- Database schemas, data types, and constraints
- Primary and foreign keys for data relationships
- SQL joins and complex queries
- Database indexes for performance optimization
- Integrating SQL with Python
- Security concerns: race conditions and SQL injection attacks

---

## 📑 Table of Contents

1. [Introduction to SQL](#-introduction-to-sql)
2. [Flat-File Databases](#-flat-file-databases)
3. [Relational Databases](#-relational-databases)
4. [SQL Fundamentals](#-sql-fundamentals)
5. [Database Schema and Design](#-database-schema-and-design)
6. [Advanced Queries and Joins](#-advanced-queries-and-joins)
7. [Indexes and Performance](#-indexes-and-performance)
8. [Python and SQL Integration](#-python-and-sql-integration)
9. [Security Considerations](#-security-considerations)

---

## 🚀 Introduction to SQL

**SQL (Structured Query Language)** is a **domain-specific language** designed specifically for managing and querying data in relational databases.

### 🎯 Why SQL?

**Problem**: Python requires significant code to analyze data:
- 14+ lines just to count favorite languages
- Manual dictionary management
- Explicit sorting logic
- Time-consuming for simple questions

**Solution**: SQL reduces complex data analysis to single-line queries!

### 📊 CRUD Operations

SQL supports four fundamental operations:

| Operation | SQL Commands | Purpose |
|-----------|-------------|---------|
| **C**reate | `CREATE`, `INSERT` | Add data to database |
| **R**ead | `SELECT` | Retrieve data from database |
| **U**pdate | `UPDATE` | Modify existing data |
| **D**elete | `DELETE`, `DROP` | Remove data from database |

---

## 📁 Flat-File Databases

**Flat-file databases** are the simplest form of data storage using plain text files.

### 📋 CSV Format (Comma-Separated Values)

**Structure:**
```csv
Timestamp,language,problem
10/28/2024 9:45:23,Python,Fiftyville
10/28/2024 9:46:12,C,Mario
10/28/2024 9:47:05,Python,"Hello, World"
```

**Key Features:**
- ✅ First row contains column headers
- ✅ Commas separate column values
- ✅ Quotes protect commas within data
- ✅ Universal format (Excel, Google Sheets, Numbers)

### 🐍 Python CSV Processing

#### Basic CSV Reading
```python
import csv

with open("favorites.csv", "r") as file:
    reader = csv.reader(file)
    next(reader)  # Skip header row
    
    for row in reader:
        print(row[1])  # Print language column
```

#### Dictionary Reader (More Robust)
```python
import csv

with open("favorites.csv", "r") as file:
    reader = csv.DictReader(file)
    
    for row in reader:
        print(row["language"])  # Access by column name
```

**🎯 Advantage**: Column order changes won't break your code!

### 📊 Counting with Dictionaries

**Goal**: Count occurrences of each language

```python
import csv

counts = {}

with open("favorites.csv", "r") as file:
    reader = csv.DictReader(file)
    
    for row in reader:
        favorite = row["language"]
        
        if favorite in counts:
            counts[favorite] += 1
        else:
            counts[favorite] = 1

# Sort by count (descending)
for language in sorted(counts, key=counts.get, reverse=True):
    print(f"{language}: {counts[language]}")
```

**Output:**
```
Python: 243
C: 59
Scratch: 11
```

### ⚠️ Limitations of Flat-Files

- ❌ No built-in query language
- ❌ Entire file must be loaded into memory
- ❌ Inefficient for large datasets
- ❌ No data relationships
- ❌ Manual sorting and filtering

---

## 🗄️ Relational Databases

**Relational databases** organize data into **tables** with **rows** and **columns**, enabling relationships between different data entities.

### 🔧 SQLite Setup

**SQLite** is a lightweight, file-based SQL database.

#### Creating a Database
```bash
sqlite3 favorites.db
```

#### Importing CSV Data
```sql
.mode csv
.import favorites.csv favorites
```

#### Viewing the Schema
```sql
.schema
```

**Output:**
```sql
CREATE TABLE favorites(
  "Timestamp" TEXT,
  "language" TEXT,
  "problem" TEXT
);
```

### 📊 Basic SQL Queries

#### Select All Data
```sql
SELECT * FROM favorites;
```

#### Select Specific Column
```sql
SELECT language FROM favorites;
```

#### Count Total Rows
```sql
SELECT COUNT(*) FROM favorites;
```

#### Count Unique Values
```sql
SELECT COUNT(DISTINCT language) FROM favorites;
```

**Output:** `3` (Scratch, C, Python)

---

## 🔍 SQL Fundamentals

### 🎯 WHERE Clause (Filtering)

**Filter by single condition:**
```sql
SELECT COUNT(*) 
FROM favorites 
WHERE language = 'C';
```

**Multiple conditions (AND):**
```sql
SELECT COUNT(*) 
FROM favorites 
WHERE language = 'C' AND problem = 'Mario';
```

**Multiple conditions (OR):**
```sql
SELECT COUNT(*) 
FROM favorites 
WHERE language = 'C' 
  AND (problem = 'Mario' OR problem = 'Scratch');
```

### 🔤 Pattern Matching with LIKE

**Wildcard operator:** `%` matches any characters

```sql
SELECT COUNT(*) 
FROM favorites 
WHERE problem LIKE 'Hello, %';
```

**Matches:**
- "Hello, World"
- "Hello, It's Me"
- Any string starting with "Hello, "

### 📊 GROUP BY (Aggregation)

**Count occurrences of each language:**
```sql
SELECT language, COUNT(*) 
FROM favorites 
GROUP BY language;
```

**Output:**
```
C|59
Python|243
Scratch|11
```

### 🔢 ORDER BY (Sorting)

**Sort by count (descending):**
```sql
SELECT language, COUNT(*) AS n
FROM favorites
GROUP BY language
ORDER BY n DESC;
```

**Using alias `AS` for readability:**
- `AS n` creates an alias for `COUNT(*)`
- Can reference alias in `ORDER BY` clause

### 🎯 LIMIT (Restricting Results)

**Get top result only:**
```sql
SELECT language, COUNT(*) AS n
FROM favorites
GROUP BY language
ORDER BY n DESC
LIMIT 1;
```

**Output:**
```
Python|243
```

**🎉 Achievement**: Reduced 14 lines of Python to 1 SQL query!

---

## 🏗️ Database Schema and Design

### 📐 Schema Definition

A **schema** defines the structure of your database:
- Tables and their relationships
- Column names and data types
- Constraints and keys

### 📊 SQLite Data Types

| Type | Description | Examples |
|------|-------------|----------|
| `INTEGER` | Whole numbers | `1`, `42`, `-5` |
| `NUMERIC` | Generic numbers, dates | `2024`, dates, years |
| `REAL` | Floating-point | `3.14`, `98.6` |
| `TEXT` | Strings | `"Hello"`, `"Python"` |
| `BLOB` | Binary data | Images, files |

### 🔒 Constraints

**Constraints** enforce data integrity:

| Constraint | Purpose | Example |
|------------|---------|---------|
| `NOT NULL` | Column must have a value | `title TEXT NOT NULL` |
| `UNIQUE` | All values must be distinct | `email TEXT UNIQUE` |
| `PRIMARY KEY` | Unique identifier for rows | `id INTEGER PRIMARY KEY` |
| `FOREIGN KEY` | Links to another table | `show_id INTEGER REFERENCES shows(id)` |

### 🗝️ Primary and Foreign Keys

#### Primary Key
**Uniquely identifies** each row in a table.

```sql
CREATE TABLE shows (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    year NUMERIC,
    episodes INTEGER
);
```

#### Foreign Key
**Establishes relationships** between tables.

```sql
CREATE TABLE ratings (
    show_id INTEGER NOT NULL REFERENCES shows(id),
    rating REAL NOT NULL,
    votes INTEGER NOT NULL
);
```

**🔗 Relationship**: Each rating belongs to exactly one show.

### 🎭 Database Design Example: IMDb

**Shows Database Structure:**

```
┌─────────────┐
│   SHOWS     │
├─────────────┤
│ id (PK)     │
│ title       │
│ year        │
│ episodes    │
└─────────────┘
       ↑
       │ (one-to-one)
       │
┌─────────────┐
│  RATINGS    │
├─────────────┤
│ show_id(FK) │
│ rating      │
│ votes       │
└─────────────┘

       ↑
       │ (one-to-many)
       │
┌─────────────┐
│   GENRES    │
├─────────────┤
│ show_id(FK) │
│ genre       │
└─────────────┘
```

**Relationship Types:**
1. **One-to-One**: Each show has one rating
2. **One-to-Many**: Each show can have multiple genres
3. **Many-to-Many**: Shows and actors (requires join table)

### 🔗 Many-to-Many Relationships

**Problem**: A show has multiple stars; an actor stars in multiple shows.

**Solution**: **Join table** (intersection table)

```sql
CREATE TABLE people (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    birth NUMERIC
);

CREATE TABLE stars (
    show_id INTEGER NOT NULL REFERENCES shows(id),
    person_id INTEGER NOT NULL REFERENCES people(id)
);
```

**Visual:**
```
┌─────────┐         ┌─────────┐         ┌─────────┐
│  SHOWS  │────────>│  STARS  │<────────│  PEOPLE │
│         │         │         │         │         │
│ id (PK) │         │show_id  │         │ id (PK) │
│ title   │         │person_id│         │ name    │
└─────────┘         └─────────┘         └─────────┘
```

---

## 🔄 Data Manipulation

### ➕ INSERT (Create)

**Add new row:**
```sql
INSERT INTO favorites (language, problem) 
VALUES ('SQL', 'Fiftyville');
```

**⚠️ Result**: `Timestamp` will be `NULL` (no value provided)

### 🗑️ DELETE (Remove)

**Delete specific rows:**
```sql
DELETE FROM favorites 
WHERE Timestamp IS NULL;
```

**⚠️ WARNING**: Without `WHERE`, deletes **ALL** rows!

### 🔄 UPDATE (Modify)

**Update specific rows:**
```sql
UPDATE favorites 
SET language = 'SQL', problem = 'Fiftyville'
WHERE language = 'C' AND problem = 'Mario';
```

**⚠️ DANGER**: Without `WHERE`, updates **ALL** rows!

```sql
-- DESTRUCTIVE: Updates ALL rows!
UPDATE favorites 
SET language = 'SQL';
```

---

## 🔗 Advanced Queries and Joins

### 🎯 Subqueries (Nested Queries)

**Find shows with rating ≥ 6.0:**
```sql
SELECT title 
FROM shows 
WHERE id IN (
    SELECT show_id 
    FROM ratings 
    WHERE rating >= 6.0
);
```

**Find genres for "Catweazle":**
```sql
SELECT genre 
FROM genres 
WHERE show_id = (
    SELECT id 
    FROM shows 
    WHERE title = 'Catweazle'
);
```

### 🔗 JOIN Operations

**Basic JOIN syntax:**
```sql
SELECT title, rating 
FROM shows 
JOIN ratings ON shows.id = ratings.show_id
WHERE rating >= 6.0
LIMIT 10;
```

**Multiple JOINs:**
```sql
SELECT title, genre 
FROM shows
JOIN genres ON shows.id = genres.show_id
WHERE title = 'Catweazle';
```

### 🎭 Complex Query: Find All Shows for an Actor

#### Method 1: Nested Queries
```sql
SELECT title 
FROM shows 
WHERE id IN (
    SELECT show_id 
    FROM stars 
    WHERE person_id = (
        SELECT id 
        FROM people 
        WHERE name = 'Steve Carell'
    )
);
```

#### Method 2: Explicit JOINs (Preferred)
```sql
SELECT title 
FROM shows
JOIN stars ON shows.id = stars.show_id
JOIN people ON stars.person_id = people.id
WHERE people.name = 'Steve Carell';
```

#### Method 3: Implicit JOINs (Older Style)
```sql
SELECT title 
FROM shows, stars, people
WHERE shows.id = stars.show_id
  AND stars.person_id = people.id
  AND people.name = 'Steve Carell';
```

**🎯 Best Practice**: Use explicit `JOIN` syntax for clarity.

---

## ⚡ Indexes and Performance

### 🐢 The Performance Problem

**Without indexes:**
```sql
SELECT title FROM shows WHERE title = 'The Office';
```
⏱️ **Time**: ~0.2 seconds (scanning 150,000+ rows)

### 📚 What Are Indexes?

**Indexes** are data structures (typically **B-trees**) that speed up data retrieval.

**Analogy**: Like a book's index pointing to page numbers

### 🚀 Creating Indexes

**Syntax:**
```sql
CREATE INDEX index_name ON table_name (column_name);
```

**Examples:**
```sql
CREATE INDEX title_index ON shows (title);
CREATE INDEX name_index ON people (name);
CREATE INDEX person_id_index ON stars (person_id);
```

### ⚡ Performance Comparison

**Before index:**
```sql
.timer ON
SELECT * FROM shows WHERE title = 'The Office';
```
⏱️ **Time**: 0.2 seconds

**After index:**
```sql
CREATE INDEX title_index ON shows (title);
SELECT * FROM shows WHERE title = 'The Office';
```
⏱️ **Time**: 0.001 seconds (200× faster!)

### ⚖️ Index Trade-offs

| Pros | Cons |
|------|------|
| ✅ Much faster `SELECT` queries | ❌ More storage space |
| ✅ Speeds up `WHERE` clauses | ❌ Slower `INSERT` operations |
| ✅ Speeds up `JOIN` operations | ❌ Slower `UPDATE` operations |
| ✅ Speeds up `ORDER BY` | ❌ Slower `DELETE` operations |

**🎯 Best Practice**: Create indexes on columns frequently used in:
- `WHERE` clauses
- `JOIN` conditions
- `ORDER BY` clauses

---

## 🐍 Python and SQL Integration

### 🔗 CS50 SQL Library

**Connecting to database:**
```python
from cs50 import SQL

# Connect to SQLite database
db = SQL("sqlite:///favorites.db")
```

### 📊 Executing Queries

**Basic query:**
```python
favorite = input("Favorite: ")

# Execute query with placeholder
rows = db.execute(
    "SELECT COUNT(*) AS n FROM favorites WHERE language = ?",
    favorite
)

# Access results
if rows:
    row = rows[0]
    print(row["n"])
```

**🔑 Key Concepts:**
- `db.execute()` returns a **list of dictionaries**
- Each dictionary represents a row
- Keys are column names
- `?` is a **placeholder** for safe parameter substitution

### 🎯 Query Results

**Return format:**
```python
rows = db.execute("SELECT language, COUNT(*) AS n FROM favorites GROUP BY language")

# rows is a list of dictionaries:
# [
#   {"language": "Python", "n": 243},
#   {"language": "C", "n": 59},
#   {"language": "Scratch", "n": 11}
# ]

for row in rows:
    print(f"{row['language']}: {row['n']}")
```

### 🔄 Complete Example

```python
from cs50 import SQL

# Connect to database
db = SQL("sqlite:///favorites.db")

# Get user input
favorite = input("What's your favorite language? ")

# Query database
rows = db.execute(
    "SELECT COUNT(*) AS n FROM favorites WHERE language = ?",
    favorite
)

# Display results
if rows:
    count = rows[0]["n"]
    print(f"{count} people prefer {favorite}")
else:
    print("No results found")
```

---

## 🔒 Security Considerations

### ⚠️ Race Conditions

**Definition**: When multiple operations on shared data occur simultaneously, leading to unexpected results.

#### 🏠 Real-World Analogy

**Scenario**: Two roommates checking for milk

```
Roommate 1: Opens fridge, sees no milk
Roommate 2: Opens fridge, sees no milk
Roommate 1: Goes to store, buys milk
Roommate 2: Goes to store, buys milk
Result: Two cartons of milk (waste!)
```

#### 💻 Database Example

**Problem**: Like button incrementation

```python
# User 1:
likes = db.execute("SELECT likes FROM posts WHERE id = ?", post_id)[0]["likes"]
# likes = 100

# User 2:
likes = db.execute("SELECT likes FROM posts WHERE id = ?", post_id)[0]["likes"]
# likes = 100

# User 1:
db.execute("UPDATE posts SET likes = ? WHERE id = ?", likes + 1, post_id)
# Sets likes to 101

# User 2:
db.execute("UPDATE posts SET likes = ? WHERE id = ?", likes + 1, post_id)
# Sets likes to 101 (should be 102!)
```

**Result**: One like is lost!

#### ✅ Solution: Transactions

**Atomic operations** ensure all-or-nothing execution:

```sql
BEGIN TRANSACTION;
    UPDATE posts SET likes = likes + 1 WHERE id = ?;
COMMIT;
```

**Alternative SQL solution:**
```sql
-- Single atomic operation
UPDATE posts SET likes = likes + 1 WHERE id = ?;
```

**🎯 Key Concept**: Use atomic operations or transactions for concurrent access.

### 🛡️ SQL Injection Attacks

**Definition**: Malicious SQL code injected through user input to manipulate database queries.

#### 🎯 The Attack

**Vulnerable Python code:**
```python
username = input("Username: ")
password = input("Password: ")

# DANGEROUS: String interpolation
query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
rows = db.execute(query)
```

**Attacker input:**
```
Username: malan@harvard.edu' --
Password: [anything]
```

**Resulting query:**
```sql
SELECT * FROM users 
WHERE username = 'malan@harvard.edu' -- ' AND password = 'anything'
```

**💥 Impact**: 
- `--` comments out the password check
- Attacker logs in without knowing password!

#### 🎨 Famous Examples

**xkcd Comic: "Little Bobby Tables"**
```
Student name: Robert'); DROP TABLE students;--
```

**License Plate Attack:**
```
NULL; DROP TABLE DMV_RECORDS; --
```

#### ✅ Prevention: Parameterized Queries

**Secure code using placeholders:**
```python
username = input("Username: ")
password = input("Password: ")

# SAFE: Using placeholders
rows = db.execute(
    "SELECT * FROM users WHERE username = ? AND password = ?",
    username,
    password
)
```

**🎯 Key Differences:**
- ❌ **Never** use f-strings or string concatenation for SQL
- ✅ **Always** use placeholders (`?`) for user input
- ✅ Database library **sanitizes** input automatically

#### 🔒 Additional Security Measures

1. **Input Validation**
   ```python
   if not username.isalnum():
       print("Invalid username")
       exit()
   ```

2. **Prepared Statements**
   - Pre-compile SQL queries
   - Treat user input as data, not code

3. **Least Privilege Principle**
   - Database users should have minimal permissions
   - Read-only accounts for queries
   - Restricted accounts for modifications

---

## 📊 SQL Command Reference

### 🔍 Query Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `SELECT` | Retrieve data | `SELECT * FROM table` |
| `FROM` | Specify table | `FROM favorites` |
| `WHERE` | Filter rows | `WHERE language = 'Python'` |
| `GROUP BY` | Aggregate data | `GROUP BY language` |
| `ORDER BY` | Sort results | `ORDER BY count DESC` |
| `LIMIT` | Restrict rows | `LIMIT 10` |
| `JOIN` | Combine tables | `JOIN ratings ON ...` |

### 🔧 Data Manipulation

| Command | Purpose | Example |
|---------|---------|---------|
| `INSERT INTO` | Add rows | `INSERT INTO table VALUES (...)` |
| `UPDATE` | Modify rows | `UPDATE table SET col = val` |
| `DELETE` | Remove rows | `DELETE FROM table WHERE ...` |
| `DROP` | Delete table | `DROP TABLE table_name` |

### 📊 Functions

| Function | Purpose | Example |
|----------|---------|---------|
| `COUNT()` | Count rows | `COUNT(*)` |
| `AVG()` | Average value | `AVG(rating)` |
| `SUM()` | Sum values | `SUM(votes)` |
| `MIN()` | Minimum value | `MIN(year)` |
| `MAX()` | Maximum value | `MAX(episodes)` |
| `DISTINCT` | Unique values | `COUNT(DISTINCT language)` |

---

## 💡 Best Practices

### ✅ Query Optimization

1. **Use indexes** on frequently queried columns
2. **SELECT specific columns** instead of `*`
3. **Use LIMIT** when you don't need all results
4. **Avoid subqueries** when JOINs are clearer
5. **Create proper indexes** for JOIN conditions

### 🔒 Security

1. **Always use parameterized queries** (placeholders)
2. **Never trust user input**
3. **Use transactions** for critical operations
4. **Implement proper authentication**
5. **Limit database user permissions**

### 📐 Database Design

1. **Normalize data** (eliminate redundancy)
2. **Use appropriate data types**
3. **Define NOT NULL** for required fields
4. **Create foreign keys** to enforce relationships
5. **Name tables and columns clearly**

---

## 🎯 Key Takeaways

### 1️⃣ **SQL is a Specialized Tool**

Python is powerful, but SQL is **purpose-built** for databases:
- More concise queries
- Better performance
- Standardized across systems

### 2️⃣ **Database Design Matters**

Good schema design:
- Eliminates data redundancy
- Enforces data integrity
- Improves query performance
- Scales better

### 3️⃣ **Indexes Speed Up Reads**

**Trade-off**:
- ⚡ Faster queries
- 🐌 Slower writes
- 💾 More storage

**Use when**: Frequently querying/filtering on a column

### 4️⃣ **Security is Critical**

Two major threats:
- **Race conditions**: Use transactions
- **SQL injection**: Use parameterized queries

### 5️⃣ **Language Integration**

SQL works alongside other languages:
- Python for logic
- SQL for data
- Best of both worlds!

---

## 🎬 Looking Ahead

### Week 8: Web Programming

SQL databases power most web applications:

```python
from flask import Flask
from cs50 import SQL

app = Flask(__name__)
db = SQL("sqlite:///database.db")

@app.route("/")
def index():
    rows = db.execute("SELECT * FROM posts ORDER BY timestamp DESC")
    return render_template("index.html", posts=rows)
```

### Real-World Applications

**E-commerce:**
- Product catalogs
- User accounts
- Order history

**Social Media:**
- User profiles
- Posts and comments
- Friend relationships

**Banking:**
- Account balances
- Transaction history
- Customer data

---

## 📚 Additional Resources

- **SQLite Documentation**: [sqlite.org](https://sqlite.org)
- **SQL Tutorial**: [w3schools.com/sql](https://w3schools.com/sql)
- **Database Design**: Normalization principles
- **CS50 SQL Library**: [cs50.readthedocs.io](https://cs50.readthedocs.io)

---

## 🎓 Practice Problems

### 🔰 Beginner

1. **Count unique problems** in favorites
2. **Find all C fans** who like Mario
3. **Sort languages** alphabetically

### 🔸 Intermediate

4. **Find all shows** with rating > 8.0
5. **List all actors** in "The Office"
6. **Count shows per genre**

### 🔥 Advanced

7. **Find common co-stars** of two actors
8. **Calculate average rating** by genre
9. **Implement full-text search** with indexes

---

## 🎬 Lecture Credits

**Instructor**: David J. Malan  
**University**: Harvard University  
**Course**: CS50x 2025  
**Video**: CS50x 2025 - Lecture 7 - SQL

---

## 📝 SQL vs Python Comparison

**Same Task: Count Favorite Languages**

**Python (14 lines):**
```python
import csv

counts = {}
with open("favorites.csv") as file:
    reader = csv.DictReader(file)
    for row in reader:
        favorite = row["language"]
        if favorite in counts:
            counts[favorite] += 1
        else:
            counts[favorite] = 1

for language in sorted(counts, key=counts.get, reverse=True):
    print(f"{language}: {counts[language]}")
```

**SQL (1 line):**
```sql
SELECT language, COUNT(*) AS n FROM favorites GROUP BY language ORDER BY n DESC;
```

**🎯 Winner**: SQL for database operations!

---

*Made with 🗄️ for CS50 students learning databases and SQL*
