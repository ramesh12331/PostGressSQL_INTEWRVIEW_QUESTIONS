Absolutely 👍 Since your **Saturday mock is specifically on PostgreSQL**, prepare these topics. The concepts are mostly standard SQL, but I'll focus on **PostgreSQL syntax and interview-style questions**.

# 🐘 PostgreSQL Mock Interview Preparation

### 📚 Topics

| #  | Topic                   | Priority | What to Prepare                                                                            |
| -- | ----------------------- | -------- | ------------------------------------------------------------------------------------------ |
| 1  | **Data Types**          | ⭐⭐⭐      | `INTEGER`, `VARCHAR`, `TEXT`, `NUMERIC`, `DATE`, `TIMESTAMP`, `BOOLEAN`, `SERIAL`/identity |
| 2  | **Constraints**         | ⭐⭐⭐      | `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL`, `CHECK`, `DEFAULT`                     |
| 3  | **DQL**                 | ⭐⭐⭐⭐⭐    | `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT`, `DISTINCT`                   |
| 4  | **Aggregate Functions** | ⭐⭐⭐⭐⭐    | `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`                                              |
| 5  | **Subqueries**          | ⭐⭐⭐⭐     | Subquery with `WHERE`, `IN`, `EXISTS`, aggregate subqueries                                |
| 6  | **Joins**               | ⭐⭐⭐⭐⭐    | `INNER`, `LEFT`, `RIGHT`, `FULL`, `CROSS`, `SELF JOIN`                                     |
| 7  | **Window Functions**    | ⭐⭐⭐⭐⭐    | `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `OVER()`, `PARTITION BY`                         |
| 8  | **DML**                 | ⭐⭐⭐⭐     | `INSERT`, `UPDATE`, `DELETE`                                                               |
| 9  | **DDL**                 | ⭐⭐⭐⭐     | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`                                                      |
| 10 | **TQL**                 | ⭐⭐⭐⭐     | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT`                                                 |
| 11 | **DCL**                 | ⭐⭐⭐      | `GRANT`, `REVOKE`                                                                          |

---

# 🎯 1. Data Types

### Must Know

| Data Type      | Meaning                   | Example                |
| -------------- | ------------------------- | ---------------------- |
| `INTEGER`      | Whole number              | `age INTEGER`          |
| `BIGINT`       | Large whole number        | `id BIGINT`            |
| `VARCHAR(n)`   | Limited-length text       | `name VARCHAR(50)`     |
| `TEXT`         | Text of variable length   | `description TEXT`     |
| `NUMERIC(p,s)` | Exact decimal             | `salary NUMERIC(10,2)` |
| `BOOLEAN`      | True/False                | `active BOOLEAN`       |
| `DATE`         | Date                      | `dob DATE`             |
| `TIMESTAMP`    | Date + time               | `created_at TIMESTAMP` |
| `SERIAL`       | Auto-incrementing integer | `id SERIAL`            |

### Interview question

**Q: What is the difference between VARCHAR and TEXT in PostgreSQL?**

**Answer:** `VARCHAR(n)` can specify a maximum length, while `TEXT` does not require a length limit.

---

# 🔐 2. Constraints

```sql
CREATE TABLE students (
    student_id INTEGER PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INTEGER CHECK (age >= 18),
    city VARCHAR(50) DEFAULT 'Hyderabad'
);
```

### Must remember

```text
PRIMARY KEY → uniquely identifies a row
FOREIGN KEY → connects two tables
UNIQUE      → prevents duplicate values
NOT NULL    → value is required
CHECK       → validates a condition
DEFAULT     → provides a default value
```

---

# 🔎 3. DQL

**DQL = Data Query Language**

Main command:

```sql
SELECT
```

Example:

```sql
SELECT *
FROM employees;
```

With filtering:

```sql
SELECT name, salary
FROM employees
WHERE salary > 50000;
```

### Important DQL concepts

```text
SELECT
WHERE
DISTINCT
GROUP BY
HAVING
ORDER BY
LIMIT
```

### ⭐ Execution order

```text
FROM
 ↓
JOIN
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
DISTINCT
 ↓
ORDER BY
 ↓
LIMIT
```

**Must remember:**

> `WHERE` → filters rows
> `GROUP BY` → creates groups
> `HAVING` → filters groups
> `ORDER BY` → sorts results

---

# 📊 4. Aggregate Functions

| Function  | Meaning               |
| --------- | --------------------- |
| `COUNT()` | Number of rows/values |
| `SUM()`   | Total                 |
| `AVG()`   | Average               |
| `MIN()`   | Minimum               |
| `MAX()`   | Maximum               |

Example:

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS average_salary,
    MAX(salary) AS highest_salary
FROM employees
GROUP BY department;
```

### Important difference

**WHERE + GROUP BY + HAVING**

```sql
SELECT department,
       SUM(salary) AS total_salary
FROM employees
WHERE city = 'Hyderabad'
GROUP BY department
HAVING SUM(salary) > 100000;
```

Flow:

```text
WHERE → Filter rows
   ↓
GROUP BY → Create groups
   ↓
HAVING → Filter groups
```

---

# 🔍 5. Subquery

A **subquery is a query inside another query**.

Example:

```sql
SELECT name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Meaning:

```text
Inner query
    ↓
Find average salary
    ↓
Outer query
    ↓
Find employees earning above average
```

### Important subquery concepts

```text
WHERE + Subquery
IN + Subquery
EXISTS + Subquery
Aggregate + Subquery
```

---

# 🔗 6. Joins

This is **very important for your mock**.

Suppose:

```text
students
---------
student_id
name
course_id

courses
---------
course_id
course_name
```

### INNER JOIN

Returns matching records from both tables.

```sql
SELECT s.name, c.course_name
FROM students s
INNER JOIN courses c
    ON s.course_id = c.course_id;
```

### LEFT JOIN

Returns **all rows from the left table** and matching rows from the right.

```sql
SELECT s.name, c.course_name
FROM students s
LEFT JOIN courses c
    ON s.course_id = c.course_id;
```

### Join memory

| Join         | Remember                  |
| ------------ | ------------------------- |
| `INNER JOIN` | Matching rows             |
| `LEFT JOIN`  | All left + matching right |
| `RIGHT JOIN` | All right + matching left |
| `FULL JOIN`  | All rows from both        |
| `CROSS JOIN` | Every combination         |
| `SELF JOIN`  | Table joined with itself  |

---

# 🪟 7. Window Functions

This is another **high-priority topic**.

### Basic syntax

```sql
function() OVER (
    PARTITION BY column
    ORDER BY column
)
```

Example:

```sql
SELECT
    name,
    department,
    salary,
    RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS salary_rank
FROM employees;
```

Meaning:

> Rank employees based on salary **within each department**.

### Must know

| Function         | Meaning                            |
| ---------------- | ---------------------------------- |
| `ROW_NUMBER()`   | Unique sequential number           |
| `RANK()`         | Same rank for ties, gaps can occur |
| `DENSE_RANK()`   | Same rank for ties, no gaps        |
| `SUM() OVER()`   | Running/partitioned total          |
| `AVG() OVER()`   | Window average                     |
| `COUNT() OVER()` | Window count                       |

### ⭐ Important difference

Suppose salaries are:

```text
100000
100000
90000
80000
```

| Function       | Result     |
| -------------- | ---------- |
| `ROW_NUMBER()` | 1, 2, 3, 4 |
| `RANK()`       | 1, 1, 3, 4 |
| `DENSE_RANK()` | 1, 1, 2, 3 |

---

# ✏️ 8. DML

**DML = Data Manipulation Language**

| Command  | Purpose     |
| -------- | ----------- |
| `INSERT` | Add data    |
| `UPDATE` | Modify data |
| `DELETE` | Delete data |

Example:

```sql
INSERT INTO students (student_id, name, age)
VALUES (1, 'Ramesh', 25);
```

```sql
UPDATE students
SET age = 26
WHERE student_id = 1;
```

```sql
DELETE FROM students
WHERE student_id = 1;
```

⚠️ **Always be careful with `UPDATE` and `DELETE` without `WHERE`.**

---

# 🏗️ 9. DDL

**DDL = Data Definition Language**

| Command    | Purpose               |
| ---------- | --------------------- |
| `CREATE`   | Create object         |
| `ALTER`    | Modify structure      |
| `DROP`     | Remove object         |
| `TRUNCATE` | Remove all table rows |

Example:

```sql
CREATE TABLE employees (
    id INTEGER PRIMARY KEY,
    name VARCHAR(50)
);
```

Add a column:

```sql
ALTER TABLE employees
ADD COLUMN salary NUMERIC(10,2);
```

---

# 🔥 DROP vs TRUNCATE vs DELETE

This is a **very common interview question**.

| Command    | What happens             |
| ---------- | ------------------------ |
| `DELETE`   | Deletes rows             |
| `TRUNCATE` | Removes all rows         |
| `DROP`     | Removes the table/object |

Memory:

```text
DELETE    → Remove selected/all rows
TRUNCATE  → Empty table
DROP      → Remove table
```

---

# 🔄 10. TQL

TQL is commonly used to refer to **Transaction Control Language (TCL)**.

Important PostgreSQL transaction commands:

```sql
BEGIN;

UPDATE employees
SET salary = salary + 5000
WHERE department = 'IT';

COMMIT;
```

If you want to undo the uncommitted transaction:

```sql
ROLLBACK;
```

### SAVEPOINT

```sql
BEGIN;

UPDATE employees
SET salary = salary + 5000;

SAVEPOINT sp1;

DELETE FROM employees
WHERE id = 10;

ROLLBACK TO sp1;

COMMIT;
```

### Remember

```text
BEGIN     → Start transaction
COMMIT    → Save changes
ROLLBACK  → Undo uncommitted changes
SAVEPOINT → Create rollback point
```

---

# 🔑 11. DCL

**DCL = Data Control Language**

Main commands:

```text
GRANT
REVOKE
```

### GRANT

Give permission.

```sql
GRANT SELECT ON employees TO user1;
```

### REVOKE

Remove permission.

```sql
REVOKE SELECT ON employees FROM user1;
```

Memory:

> **GRANT → Give permission**
> **REVOKE → Remove permission**

---

# 🧠 PostgreSQL Mock — Must Remember Table

| Concept         | One-line Memory                                                    |
| --------------- | ------------------------------------------------------------------ |
| Data Type       | Defines what kind of data can be stored                            |
| PRIMARY KEY     | Unique row identification                                          |
| FOREIGN KEY     | Relationship between tables                                        |
| UNIQUE          | No duplicate values                                                |
| NOT NULL        | Value required                                                     |
| CHECK           | Condition validation                                               |
| DEFAULT         | Default value                                                      |
| DQL             | Read/query data                                                    |
| Aggregate       | Calculate summary                                                  |
| Subquery        | Query inside query                                                 |
| JOIN            | Combine tables                                                     |
| Window Function | Calculation across related rows without grouping them into one row |
| DML             | Insert/Update/Delete data                                          |
| DDL             | Create/modify/delete structure                                     |
| TQL/TCL         | Manage transactions                                                |
| DCL             | Manage permissions                                                 |

# 🎯 Highest-Priority Interview Questions

Before Saturday, make sure you can answer these **without looking at notes**:

1. What are data types in PostgreSQL?
2. What is a primary key?
3. Primary key vs unique?
4. What is a foreign key?
5. What is `NOT NULL`?
6. What is `CHECK`?
7. What is DQL?
8. `WHERE` vs `HAVING`?
9. What is `GROUP BY`?
10. What are aggregate functions?
11. `COUNT(*)` vs `COUNT(column)`?
12. What is a subquery?
13. What is a correlated subquery?
14. What is a JOIN?
15. INNER JOIN vs LEFT JOIN?
16. LEFT JOIN vs RIGHT JOIN?
17. What is a SELF JOIN?
18. What is a window function?
19. `ROW_NUMBER()` vs `RANK()`?
20. `RANK()` vs `DENSE_RANK()`?
21. What is `PARTITION BY`?
22. DDL vs DML?
23. DELETE vs TRUNCATE?
24. TRUNCATE vs DROP?
25. What is a transaction?
26. COMMIT vs ROLLBACK?
27. What is SAVEPOINT?
28. What is DCL?
29. GRANT vs REVOKE?
30. Explain SQL logical execution order.

### 🏆 One-line revision

```text
DATA TYPES  → What type of data?
CONSTRAINTS → What rules?
DQL         → How to read?
AGGREGATE   → How to calculate?
SUBQUERY    → Query inside query?
JOINS       → How to combine tables?
WINDOW      → Calculate across rows?
DML         → How to change data?
DDL         → How to change structure?
TQL/TCL     → How to control transactions?
DCL         → How to control permissions?
```

For your mock, **Joins + Window Functions + GROUP BY/HAVING + Subqueries** deserve extra practice because they require you to actually write queries, not just remember definitions.
----
----
Yes 👍 For a **beginner PostgreSQL mock interview**, I would learn these 30 questions in a very simple pattern:

> **First understand the meaning → then remember one example → then learn the interview answer.**

Don't memorize long definitions. In an interview, give a **2–3 sentence answer**, and if they ask further, explain with an example.

# 🐘 PostgreSQL Mock Interview — Beginner Friendly

## 1. What are Data Types in PostgreSQL?

### 🧠 Easy understanding

A **data type tells PostgreSQL what kind of value we are going to store** in a column.

Think:

```text
name   → TEXT
age    → INTEGER
salary → NUMERIC
dob    → DATE
active → BOOLEAN
```

### Example

```sql
CREATE TABLE employees (
    name TEXT,
    age INTEGER,
    salary NUMERIC(10,2),
    dob DATE,
    active BOOLEAN
);
```

### 🎤 Interview Answer

> **Data types define what kind of data can be stored in a column. PostgreSQL provides data types such as INTEGER, VARCHAR, TEXT, NUMERIC, DATE, TIMESTAMP, and BOOLEAN.**

**Memory:**
`Data Type = What type of data?`

---

# 2. What is a Primary Key?

### 🧠 Easy understanding

A **Primary Key uniquely identifies each row** in a table.

Imagine student IDs:

```text
student_id
-----------
1
2
3
```

Each student has a unique ID.

```sql
student_id INTEGER PRIMARY KEY
```

### Important rules

A primary key:

* Cannot contain `NULL`
* Must be unique
* Identifies a row

### 🎤 Interview Answer

> **A primary key is a column or combination of columns that uniquely identifies each row in a table. It cannot contain NULL values and must be unique.**

**Memory:**
`PRIMARY KEY = Unique identity`

---

# 3. Primary Key vs UNIQUE

### 🧠 Easy understanding

Both help prevent duplicate values, but they have different purposes.

| PRIMARY KEY                          | UNIQUE                                           |
| ------------------------------------ | ------------------------------------------------ |
| Identifies each row                  | Prevents duplicate values                        |
| Cannot be NULL                       | PostgreSQL allows NULL values in a unique column |
| One primary-key constraint per table | Multiple UNIQUE constraints can exist            |

Example:

```sql
student_id INTEGER PRIMARY KEY,
email TEXT UNIQUE
```

Think:

```text
student_id → Who is this student?
email      → Can two students use the same email?
```

### 🎤 Interview Answer

> **A primary key uniquely identifies each row and cannot contain NULL. A UNIQUE constraint prevents duplicate values, and PostgreSQL allows NULL values in a unique column. A table can have one primary-key constraint but can have multiple UNIQUE constraints.**

**Memory:**
`PK = Identity`
`UNIQUE = No duplicate`

---

# 4. What is a Foreign Key?

### 🧠 Easy understanding

A foreign key **connects one table to another table**.

Example:

```text
students
---------
student_id
name
course_id
       ↓
courses
---------
course_id
course_name
```

`students.course_id` can reference `courses.course_id`.

```sql
course_id INTEGER REFERENCES courses(course_id)
```

### 🎤 Interview Answer

> **A foreign key is a column that references a key in another table. It is used to establish a relationship between tables and maintain referential integrity.**

**Memory:**
`FOREIGN KEY = Table connection`

---

# 5. What is NOT NULL?

### 🧠 Easy understanding

`NOT NULL` means:

> **This column must have a value.**

```sql
name TEXT NOT NULL
```

This is not allowed:

```text
name = NULL
```

### 🎤 Interview Answer

> **NOT NULL is a constraint that prevents a column from storing NULL values. It is used when a value is mandatory.**

**Memory:**
`NOT NULL = Must have value`

---

# 6. What is CHECK?

### 🧠 Easy understanding

`CHECK` is used to **enforce a condition**.

```sql
age INTEGER CHECK (age >= 18)
```

So:

```text
25 → ✅
18 → ✅
15 → ❌
```

### 🎤 Interview Answer

> **CHECK is a constraint used to ensure that column values satisfy a specified condition.**

**Memory:**
`CHECK = Condition`

---

# 7. What is DQL?

### 🧠 Easy understanding

DQL means **Data Query Language**.

Mainly:

```sql
SELECT
```

It is used to **retrieve/read data**.

```sql
SELECT *
FROM employees;
```

### 🎤 Interview Answer

> **DQL is Data Query Language. It is used to retrieve data from the database, mainly using the SELECT statement.**

**Memory:**
`DQL = Read`

---

# 8. WHERE vs HAVING

This is **very important**. ⭐⭐⭐

### 🧠 Easy understanding

Think:

```text
WHERE  → Filter individual rows
HAVING → Filter groups
```

Example:

```sql
SELECT department, COUNT(*)
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING COUNT(*) > 5;
```

Flow:

```text
WHERE
 ↓
GROUP BY
 ↓
HAVING
```

### 🎤 Interview Answer

> **WHERE filters individual rows before grouping, while HAVING filters groups after GROUP BY. HAVING is commonly used with aggregate functions.**

**Memory:**

> `WHERE = Rows`
> `HAVING = Groups`

---

# 9. What is GROUP BY?

### 🧠 Easy understanding

`GROUP BY` puts rows with the **same value into groups**.

Suppose:

```text
department
----------
IT
IT
HR
HR
Sales
```

`GROUP BY department` creates:

```text
IT
HR
Sales
```

Then we can calculate:

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

### 🎤 Interview Answer

> **GROUP BY is used to group rows with the same values, usually so that aggregate functions can be applied to each group.**

**Memory:**
`GROUP BY = Make groups`

---

# 10. What are Aggregate Functions?

### 🧠 Easy understanding

Aggregate functions **calculate a result from multiple rows**.

| Function  | Meaning  |
| --------- | -------- |
| `COUNT()` | Count    |
| `SUM()`   | Total    |
| `AVG()`   | Average  |
| `MIN()`   | Smallest |
| `MAX()`   | Largest  |

Example:

```sql
SELECT
    COUNT(*),
    SUM(salary),
    AVG(salary),
    MIN(salary),
    MAX(salary)
FROM employees;
```

### 🎤 Interview Answer

> **Aggregate functions perform calculations on multiple rows and return a single result for each group or for the entire result set.**

**Memory:**
`COUNT SUM AVG MIN MAX`

---

# 11. COUNT(*) vs COUNT(column)

### 🧠 Easy understanding

This is a common interview question.

```text
COUNT(*)       → counts rows
COUNT(column)  → counts non-NULL values in that column
```

Example:

```sql
SELECT COUNT(*)
FROM employees;
```

Counts all rows.

```sql
SELECT COUNT(email)
FROM employees;
```

Counts only rows where `email` is not NULL.

### 🎤 Interview Answer

> **COUNT(*) counts rows, including rows containing NULL values in individual columns. COUNT(column) counts only non-NULL values in that column.**

**Memory:**
`COUNT(*) = Rows`
`COUNT(column) = Non-NULL values`

---

# 12. What is a Subquery?

### 🧠 Easy understanding

A **subquery is a query inside another query**.

Think:

```text
Outer Query
    ↑
Inner Query
```

Example:

```sql
SELECT name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

First:

```text
Find average salary
```

Then:

```text
Find employees above average
```

### 🎤 Interview Answer

> **A subquery is a query written inside another SQL query. The inner query provides a result that is used by the outer query.**

**Memory:**
`Subquery = Query inside Query`

---

# 13. What is a Correlated Subquery?

### 🧠 Easy understanding

A normal subquery can work independently.

A **correlated subquery depends on the outer query**.

Think:

```text
Outer row
   ↓
Inner query checks that row
   ↓
Next outer row
   ↓
Inner query checks again
```

Example:

```sql
SELECT e1.name, e1.salary, e1.department
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department = e1.department
);
```

Here the inner query uses:

```sql
e1.department
```

from the outer query.

### 🎤 Interview Answer

> **A correlated subquery is a subquery that depends on values from the outer query. It is logically evaluated for each relevant outer row.**

**Memory:**
`Normal subquery = Independent`
`Correlated = Depends on outer query`

---

# 14. What is a JOIN?

### 🧠 Easy understanding

A JOIN is used to **combine data from multiple tables** using a related column.

Example:

```text
students              courses

student_id            course_id
course_id      →      course_name
name
```

```sql
SELECT s.name, c.course_name
FROM students s
JOIN courses c
ON s.course_id = c.course_id;
```

### 🎤 Interview Answer

> **A JOIN is used to combine rows from two or more tables based on a related column or condition.**

**Memory:**
`JOIN = Combine tables`

---

# 15. INNER JOIN vs LEFT JOIN

### 🧠 Easy understanding

```text
INNER JOIN
→ Only matching records

LEFT JOIN
→ All records from LEFT table
→ Matching records from RIGHT table
```

Example:

```sql
SELECT s.name, c.course_name
FROM students s
INNER JOIN courses c
ON s.course_id = c.course_id;
```

vs

```sql
SELECT s.name, c.course_name
FROM students s
LEFT JOIN courses c
ON s.course_id = c.course_id;
```

### 🎤 Interview Answer

> **INNER JOIN returns only matching rows from both tables. LEFT JOIN returns all rows from the left table and matching rows from the right table; unmatched right-side values appear as NULL.**

**Memory:**

`INNER = Matching`
`LEFT = All Left + Matching Right`

---

# 16. LEFT JOIN vs RIGHT JOIN

### 🧠 Easy understanding

They are basically opposite directions.

```text
LEFT JOIN
→ Keep everything from LEFT table

RIGHT JOIN
→ Keep everything from RIGHT table
```

### 🎤 Interview Answer

> **LEFT JOIN returns all rows from the left table, while RIGHT JOIN returns all rows from the right table. Both include matching rows from the other table.**

**Memory:**

`LEFT JOIN → Left important`
`RIGHT JOIN → Right important`

---

# 17. What is a SELF JOIN?

### 🧠 Easy understanding

A **table joins with itself**.

Useful for employee-manager relationships.

```text
employees

employee_id
name
manager_id
```

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees e
LEFT JOIN employees m
    ON e.manager_id = m.employee_id;
```

Here:

```text
employees e → Employee
employees m → Manager
```

Same table, different aliases.

### 🎤 Interview Answer

> **A SELF JOIN is when a table is joined with itself. It is useful for hierarchical relationships such as employee-manager relationships.**

**Memory:**
`SELF JOIN = Same table + different aliases`

---

# 18. What is a Window Function?

### 🧠 Easy understanding

A window function performs a calculation across related rows **without combining those rows into one row like GROUP BY does**.

Example:

```sql
SELECT
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;
```

Output idea:

```text
Name       Salary    Rank
Ramesh     80000       1
Raj        70000       2
John       60000       3
```

### 🎤 Interview Answer

> **A window function performs calculations across a set of related rows while keeping the individual rows in the result.**

**Memory:**
`GROUP BY → combines rows`
`Window → keeps rows`

---

# 19. ROW_NUMBER() vs RANK()

### 🧠 Easy understanding

Suppose:

```text
Salary
100000
100000
90000
```

### ROW_NUMBER

Every row gets a different number:

```text
1
2
3
```

### RANK

Same salary gets the same rank:

```text
1
1
3
```

### 🎤 Interview Answer

> **ROW_NUMBER() assigns a unique sequential number to each row. RANK() gives the same rank to tied rows and leaves gaps after ties.**

**Memory:**

`ROW_NUMBER = Always unique`
`RANK = Same rank + gaps`

---

# 20. RANK() vs DENSE_RANK()

Suppose:

```text
100000
100000
90000
80000
```

Results:

| Salary | RANK | DENSE_RANK |
| -----: | ---: | ---------: |
| 100000 |    1 |          1 |
| 100000 |    1 |          1 |
|  90000 |    3 |          2 |
|  80000 |    4 |          3 |

### 🎤 Interview Answer

> **Both RANK() and DENSE_RANK() assign the same rank to tied rows. RANK() leaves gaps after ties, while DENSE_RANK() does not.**

**Memory:**

`RANK → gaps`
`DENSE_RANK → no gaps`

---

# 21. What is PARTITION BY?

### 🧠 Easy understanding

`PARTITION BY` divides the rows into **separate groups for a window function**.

Example:

```sql
SELECT
    name,
    department,
    salary,
    RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS rank
FROM employees;
```

Meaning:

```text
IT       → rank IT employees
HR       → rank HR employees
Sales    → rank Sales employees
```

### 🎤 Interview Answer

> **PARTITION BY divides rows into groups within a window function. The window calculation is then performed separately for each group.**

**Memory:**
`PARTITION BY = Separate windows/groups`

---

# 22. DDL vs DML

### 🧠 Easy understanding

Think:

```text
DDL → Structure
DML → Data
```

### DDL

```sql
CREATE
ALTER
DROP
TRUNCATE
```

### DML

```sql
INSERT
UPDATE
DELETE
```

| DDL                        | DML           |
| -------------------------- | ------------- |
| Defines/modifies structure | Modifies data |
| CREATE                     | INSERT        |
| ALTER                      | UPDATE        |
| DROP                       | DELETE        |
| TRUNCATE                   | —             |

### 🎤 Interview Answer

> **DDL is used to define or modify database structures, while DML is used to insert, update, and delete data.**

**Memory:**
`DDL = Structure`
`DML = Data`

---

# 23. DELETE vs TRUNCATE

### 🧠 Easy understanding

```text
DELETE    → Remove rows
TRUNCATE  → Empty the table
```

### DELETE

```sql
DELETE FROM employees
WHERE department = 'HR';
```

Can remove selected rows.

### TRUNCATE

```sql
TRUNCATE TABLE employees;
```

Removes all rows from the table.

### 🎤 Interview Answer

> **DELETE is used to remove rows and can use a WHERE condition. TRUNCATE removes all rows from a table without scanning individual rows in the same way as DELETE.**

For PostgreSQL, both can participate in transactions, so avoid memorizing the oversimplified statement that **TRUNCATE can never be rolled back**.

**Memory:**
`DELETE = Selected rows`
`TRUNCATE = Empty table`

---

# 24. TRUNCATE vs DROP

Very easy if you remember:

```text
TRUNCATE → Data gone
DROP     → Table gone
```

### Example

```sql
TRUNCATE TABLE employees;
```

The table still exists.

```sql
DROP TABLE employees;
```

The table itself is removed.

### 🎤 Interview Answer

> **TRUNCATE removes all rows while keeping the table structure. DROP removes the table itself, including its structure.**

**Memory:**

`TRUNCATE = Empty table`
`DROP = Remove table`

---

# 25. What is a Transaction?

### 🧠 Easy understanding

A transaction is a **group of database operations treated as one unit of work**.

Example: Bank transfer

```text
Remove ₹1000 from A
        +
Add ₹1000 to B
```

Both operations should succeed together.

### 🎤 Interview Answer

> **A transaction is a sequence of database operations treated as a single unit of work. PostgreSQL provides transaction commands such as BEGIN, COMMIT, and ROLLBACK.**

**Memory:**
`Transaction = One unit of work`

---

# 26. COMMIT vs ROLLBACK

### 🧠 Easy understanding

```text
COMMIT
→ Save transaction changes

ROLLBACK
→ Undo uncommitted transaction changes
```

Example:

```sql
BEGIN;

UPDATE employees
SET salary = 60000
WHERE id = 1;

COMMIT;
```

Or:

```sql
BEGIN;

UPDATE employees
SET salary = 60000
WHERE id = 1;

ROLLBACK;
```

### 🎤 Interview Answer

> **COMMIT permanently makes the transaction changes part of the database state, while ROLLBACK undoes changes made in the current uncommitted transaction.**

**Memory:**
`COMMIT = Save`
`ROLLBACK = Undo`

---

# 27. What is SAVEPOINT?

### 🧠 Easy understanding

A savepoint is a **checkpoint inside a transaction**.

```text
BEGIN
  ↓
Operation 1
  ↓
SAVEPOINT
  ↓
Operation 2
  ↓
Something wrong
  ↓
ROLLBACK TO SAVEPOINT
```

Example:

```sql
BEGIN;

UPDATE employees
SET salary = salary + 5000;

SAVEPOINT sp1;

DELETE FROM employees
WHERE id = 10;

ROLLBACK TO sp1;

COMMIT;
```

### 🎤 Interview Answer

> **SAVEPOINT creates a checkpoint inside a transaction so that we can roll back to that point without rolling back the entire transaction.**

**Memory:**
`SAVEPOINT = Checkpoint`

---

# 28. What is DCL?

### 🧠 Easy understanding

DCL means **Data Control Language**.

It controls **database permissions/access**.

Main commands:

```text
GRANT
REVOKE
```

### 🎤 Interview Answer

> **DCL is Data Control Language. It is used to manage permissions and access privileges on database objects.**

**Memory:**
`DCL = Permissions`

---

# 29. GRANT vs REVOKE

### 🧠 Easy understanding

```text
GRANT  → Give permission
REVOKE → Remove permission
```

Example:

```sql
GRANT SELECT ON employees TO user1;
```

Give permission to read.

```sql
REVOKE SELECT ON employees FROM user1;
```

Remove that permission.

### 🎤 Interview Answer

> **GRANT is used to give privileges to a user or role, while REVOKE is used to remove previously granted privileges.**

**Memory:**
`GRANT = Give`
`REVOKE = Remove`

---

# 30. Explain SQL Logical Execution Order

⭐ **Very important interview question**

Don't memorize it as a difficult concept. Think about what PostgreSQL logically needs to do first.

Suppose:

```sql
SELECT department, COUNT(*)
FROM employees
WHERE salary > 30000
GROUP BY department
HAVING COUNT(*) > 5
ORDER BY COUNT(*) DESC
LIMIT 3;
```

Think like this:

### Step 1 — FROM

**Where does data come from?**

```text
FROM employees
```

### Step 2 — WHERE

**Which rows do I want?**

```text
salary > 30000
```

### Step 3 — GROUP BY

**How should I make groups?**

```text
department
```

### Step 4 — HAVING

**Which groups do I want?**

```text
COUNT(*) > 5
```

### Step 5 — SELECT

**What should I display?**

```text
department
COUNT(*)
```

### Step 6 — ORDER BY

**How should I sort?**

```text
COUNT(*) DESC
```

### Step 7 — LIMIT

**How many results do I want?**

```text
3
```

### 🧠 Beginner Memory

```text
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
 ↓
LIMIT
```

When JOIN is present, conceptually the join operation occurs as part of the `FROM` stage.

### 🎤 Interview Answer

> **The logical SQL execution order is FROM, JOIN/ON, WHERE, GROUP BY, HAVING, SELECT, DISTINCT, ORDER BY, and LIMIT. This explains why we filter rows with WHERE before grouping and filter groups with HAVING after GROUP BY.**

---

# 🔥 FINAL 30-QUESTION REVISION TABLE

This is the table I recommend you **revise every day before the mock**.

|  # | Interview Question         | Short Beginner Answer                                         |
| -: | -------------------------- | ------------------------------------------------------------- |
|  1 | Data Types?                | Define what type of data a column can store.                  |
|  2 | Primary Key?               | Uniquely identifies each row.                                 |
|  3 | PK vs UNIQUE?              | PK identifies row; UNIQUE prevents duplicates.                |
|  4 | Foreign Key?               | Connects one table to another.                                |
|  5 | NOT NULL?                  | Column cannot contain NULL.                                   |
|  6 | CHECK?                     | Ensures a condition is satisfied.                             |
|  7 | DQL?                       | Used to retrieve data, mainly SELECT.                         |
|  8 | WHERE vs HAVING?           | WHERE filters rows; HAVING filters groups.                    |
|  9 | GROUP BY?                  | Creates groups of similar values.                             |
| 10 | Aggregate Functions?       | Calculate values across multiple rows.                        |
| 11 | COUNT(*) vs COUNT(column)? | All rows vs non-NULL column values.                           |
| 12 | Subquery?                  | Query inside another query.                                   |
| 13 | Correlated Subquery?       | Inner query depends on outer query.                           |
| 14 | JOIN?                      | Combines related data from tables.                            |
| 15 | INNER vs LEFT?             | Matching rows vs all left rows + matches.                     |
| 16 | LEFT vs RIGHT?             | Keep left table vs keep right table.                          |
| 17 | SELF JOIN?                 | A table joined with itself.                                   |
| 18 | Window Function?           | Calculates across rows while keeping individual rows.         |
| 19 | ROW_NUMBER vs RANK?        | Unique numbers vs ranks with ties.                            |
| 20 | RANK vs DENSE_RANK?        | RANK has gaps; DENSE_RANK has no gaps.                        |
| 21 | PARTITION BY?              | Creates separate groups for a window calculation.             |
| 22 | DDL vs DML?                | Structure vs data.                                            |
| 23 | DELETE vs TRUNCATE?        | Delete rows vs empty entire table.                            |
| 24 | TRUNCATE vs DROP?          | Empty table vs remove table.                                  |
| 25 | Transaction?               | Group of operations treated as one unit.                      |
| 26 | COMMIT vs ROLLBACK?        | Save vs undo uncommitted changes.                             |
| 27 | SAVEPOINT?                 | Checkpoint inside a transaction.                              |
| 28 | DCL?                       | Controls database permissions.                                |
| 29 | GRANT vs REVOKE?           | Give permission vs remove permission.                         |
| 30 | SQL execution order?       | FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT. |

# 🧠 The Ultimate Beginner Memory Map

Don't memorize 30 separate answers. Connect them:

```text
                    🐘 PostgreSQL
                         |
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
    STRUCTURE          DATA             ACCESS
        |                |                |
       DDL              DML              DCL
        |                |                |
 CREATE/ALTER       INSERT/UPDATE       GRANT
 DROP/TRUNCATE      /DELETE             REVOKE
        |
        ↓
    CONSTRAINTS
        |
 PK / FK / UNIQUE / NOT NULL / CHECK
```

Then for querying:

```text
                 🔎 DQL
                   |
                 SELECT
                   |
          ┌────────┴────────┐
          ↓                 ↓
       Tables             Filter
        JOIN              WHERE
          ↓                 ↓
      Combine            Rows
                            ↓
                        GROUP BY
                            ↓
                          Groups
                            ↓
                         HAVING
                            ↓
                     Filter Groups
                            ↓
                       ORDER BY
                            ↓
                          LIMIT
```

And advanced querying:

```text
SUBQUERY
   ↓
Query inside Query

WINDOW FUNCTION
   ↓
ROW_NUMBER
RANK
DENSE_RANK
   ↓
PARTITION BY
```

### 🎤 One golden interview rule

If the interviewer asks **"Explain..."**, don't give a one-word answer.

Use this pattern:

> **Definition → Purpose → Small Example**

For example:

**Interviewer:** What is a window function?

**You:**

> "**A window function performs calculations across related rows without combining them into a single row. It is useful for ranking, running totals, and comparisons. For example, we can use RANK() OVER(PARTITION BY department ORDER BY salary DESC) to rank employees within each department.**"

That style will sound much more natural than trying to memorize textbook definitions.
