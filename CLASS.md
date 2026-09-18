Yes 👍 These are the **additional PostgreSQL topics** you need to prepare for your mock:

1. **GROUP BY + ORDER BY**
2. **SQL Joins**
3. **Window Functions**
4. **Window Functions + Subqueries**
5. **CTE (Common Table Expressions)**
6. **SQL Procedures**

For a beginner, I recommend learning them in this order:

```text
GROUP BY + ORDER BY
        ↓
JOINS
        ↓
WINDOW FUNCTIONS
        ↓
SUBQUERY + WINDOW FUNCTIONS
        ↓
CTE
        ↓
PROCEDURES
```

# 🐘 PostgreSQL Mock — Topic-wise Focus

|  # | Topic                   | What you must know                                                              |
| -: | ----------------------- | ------------------------------------------------------------------------------- |
|  1 | **GROUP BY + ORDER BY** | `GROUP BY`, `SUM`, `COUNT`, `AVG`, `HAVING`, `ORDER BY`, `ASC`, `DESC`, `LIMIT` |
|  2 | **Joins**               | `INNER`, `LEFT`, `RIGHT`, `FULL`, `CROSS`, `SELF JOIN`, `ON`                    |
|  3 | **Window Functions**    | `OVER()`, `PARTITION BY`, `ORDER BY`, `ROW_NUMBER`, `RANK`, `DENSE_RANK`        |
|  4 | **Window + Subquery**   | Subquery + ranking, highest/second-highest, department-wise calculations        |
|  5 | **CTE**                 | `WITH`, temporary named result, multiple CTEs, CTE + JOIN/window                |
|  6 | **Procedures**          | `CREATE PROCEDURE`, parameters, `CALL`, `BEGIN/END`, PL/pgSQL basics            |

---

# 🎯 1. GROUP BY + ORDER BY

### 🧠 Remember

```text
GROUP BY → Make groups
ORDER BY → Sort results
```

Example:

```sql
SELECT department,
       COUNT(*) AS employee_count
FROM employees
GROUP BY department
ORDER BY employee_count DESC;
```

Meaning:

```text
employees
   ↓
GROUP BY department
   ↓
Count each department
   ↓
ORDER BY highest count first
```

### Must-know questions

* What is `GROUP BY`?
* What is `ORDER BY`?
* `WHERE` vs `HAVING`?
* `GROUP BY` vs `ORDER BY`?
* What is `ASC`?
* What is `DESC`?
* Can we use aggregate functions with `GROUP BY`?
* How do you find the top 3 categories by sales?

---

# 🔗 2. SQL JOINS ⭐⭐⭐

### 🧠 Remember

> **JOIN = Combine data from tables**

Example:

```sql
SELECT s.name,
       c.course_name
FROM students s
INNER JOIN courses c
ON s.course_id = c.course_id;
```

Think:

```text
students                courses
   |                       |
student_id              course_id
course_id       ─────→   course_name
```

### Must know

```text
INNER JOIN → Matching rows
LEFT JOIN  → All left + matching right
RIGHT JOIN → All right + matching left
FULL JOIN  → All rows from both
CROSS JOIN → Every combination
SELF JOIN  → Same table joined with itself
```

### Interview question

**Q: What is the difference between INNER JOIN and LEFT JOIN?**

> **INNER JOIN returns only matching rows from both tables. LEFT JOIN returns all rows from the left table and matching rows from the right table.**

---

# 🪟 3. WINDOW FUNCTIONS ⭐⭐⭐

This is very important.

### 🧠 Beginner meaning

A Window Function performs a calculation across related rows **while keeping the individual rows**.

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

Think:

```text
PARTITION BY department
        ↓
Separate IT / HR / Sales
        ↓
ORDER BY salary
        ↓
Rank employees
```

### Must know

| Function         | Meaning                      |
| ---------------- | ---------------------------- |
| `ROW_NUMBER()`   | Unique number                |
| `RANK()`         | Same rank for ties + gaps    |
| `DENSE_RANK()`   | Same rank for ties + no gaps |
| `SUM() OVER()`   | Window total                 |
| `AVG() OVER()`   | Window average               |
| `COUNT() OVER()` | Window count                 |

### Most important differences

```text
ROW_NUMBER → 1, 2, 3, 4
RANK       → 1, 1, 3, 4
DENSE_RANK → 1, 1, 2, 3
```

---

# 🔍 4. Window Functions + Subquery

Here you combine two concepts.

### Example: Employees earning more than average

First find average:

```sql
SELECT AVG(salary)
FROM employees;
```

Then use it:

```sql
SELECT name, salary
FROM employees
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
);
```

Now combine with ranking:

```sql
SELECT *
FROM (
    SELECT
        name,
        department,
        salary,
        RANK() OVER (
            PARTITION BY department
            ORDER BY salary DESC
        ) AS salary_rank
    FROM employees
) x
WHERE salary_rank = 1;
```

### 🧠 Understand the flow

```text
Inner Query
     ↓
Window Function
     ↓
Create rank
     ↓
Outer Query
     ↓
Filter rank = 1
```

This pattern is **very useful in interviews**.

---

# 📦 5. CTE — Common Table Expression ⭐⭐⭐

### 🧠 Beginner meaning

A CTE is a **temporary named result that we can use inside a query**.

Think:

```text
WITH
 ↓
Create temporary result
 ↓
Use that result
```

### Syntax

```sql
WITH cte_name AS (
    SELECT ...
)
SELECT *
FROM cte_name;
```

### Example

```sql
WITH high_salary AS (
    SELECT name, salary
    FROM employees
    WHERE salary > 50000
)
SELECT *
FROM high_salary;
```

Think:

```text
employees
    ↓
Filter salary > 50000
    ↓
high_salary
    ↓
SELECT from high_salary
```

### 🎤 Interview Answer

> **A CTE, or Common Table Expression, is a temporary named result set defined using WITH. It makes complex queries easier to read, organize, and reuse within the same SQL statement.**

### Important difference

**Subquery:**

```sql
SELECT *
FROM (
    SELECT *
    FROM employees
) x;
```

**CTE:**

```sql
WITH employee_data AS (
    SELECT *
    FROM employees
)
SELECT *
FROM employee_data;
```

### 🧠 Memory

> **Subquery = Query inside query**
> **CTE = Named query result**

---

# ⚙️ 6. PostgreSQL PROCEDURES

This is a little more advanced, so don't worry if it initially feels difficult.

### 🧠 Beginner meaning

A procedure is a **stored program in the database that can be executed using `CALL`**.

Think:

```text
Procedure
    ↓
Store SQL logic
    ↓
CALL procedure
    ↓
Execute logic
```

### Basic syntax

```sql
CREATE OR REPLACE PROCEDURE procedure_name()
LANGUAGE plpgsql
AS $$
BEGIN

    -- SQL statements

END;
$$;
```

Execute:

```sql
CALL procedure_name();
```

### Example

```sql
CREATE OR REPLACE PROCEDURE update_salary()
LANGUAGE plpgsql
AS $$
BEGIN

    UPDATE employees
    SET salary = salary + 5000
    WHERE department = 'IT';

END;
$$;
```

Execute:

```sql
CALL update_salary();
```

### 🎤 Interview Answer

> **A PostgreSQL procedure is a stored database program used to perform a sequence of operations. We create it using CREATE PROCEDURE and execute it using CALL.**

### Must know

```text
CREATE PROCEDURE → Create procedure
CALL             → Execute procedure
PL/pgSQL         → PostgreSQL procedural language
BEGIN            → Start procedure body
END              → End procedure body
```

---

# 🏆 Final Mock Preparation Map

Your entire preparation can now be remembered like this:

```text
                 🐘 PostgreSQL
                       |
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      QUERY          TABLES         CONTROL
        |              |              |
        ↓              ↓              ↓
 GROUP BY           JOINS            DML
 ORDER BY           SELF JOIN        DDL
 WHERE              etc.             TQL/TCL
 HAVING                              DCL
        |
        ↓
   ADVANCED QUERY
        |
   ┌────┼────┐
   ↓    ↓    ↓
SUBQUERY WINDOW CTE
          |
          ↓
    PROCEDURES
```

# 🔥 Highest Priority

For your mock, I would practice these especially:

| Priority | Topic      | Must Know                       |
| :------: | ---------- | ------------------------------- |
|  🔥🔥🔥  | GROUP BY   | `GROUP BY + HAVING + aggregate` |
|  🔥🔥🔥  | JOIN       | INNER + LEFT + SELF             |
|  🔥🔥🔥  | Window     | `OVER + PARTITION BY + RANK`    |
|  🔥🔥🔥  | Subquery   | Average salary, second highest  |
|   🔥🔥   | CTE        | `WITH ... AS (...)`             |
|   🔥🔥   | Procedures | `CREATE PROCEDURE + CALL`       |

### 🧠 Final Golden Memory

```text
GROUP BY    → Make groups
ORDER BY    → Sort
JOIN        → Combine tables
SUBQUERY    → Query inside query
WINDOW      → Calculate while keeping rows
PARTITION   → Separate window groups
CTE         → Named temporary query result
PROCEDURE   → Stored executable database logic
```

For the mock, **don't try to memorize all the syntax at once**. First become comfortable answering **what it is, why we use it, and one simple query** for each topic.
