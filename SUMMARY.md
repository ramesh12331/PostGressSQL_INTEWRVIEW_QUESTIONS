# 📚 PostgreSQL Final Summary — Beginner Level

You have covered these **5 important SQL topics**:

1. **GROUP BY & ORDER BY**
2. **SQL JOINs**
3. **Window Functions**
4. **Window Functions + Subquery**
5. **CTE**
6. **Procedures**

> You listed `GROUP BY, ORDER BY` separately and then topics 1–5, so the complete revision actually contains **6 areas**.

I’ll give you a **beginner-friendly final summary**, focusing on **definition → purpose → syntax → example → interview point → memory trick**.

---

# 1️⃣ GROUP BY & ORDER BY

## 🔹 GROUP BY

### Definition

`GROUP BY` is used to **combine rows having the same value into groups**.

Usually we use it with aggregate functions:

```text
SUM()
COUNT()
AVG()
MIN()
MAX()
```

### Example

```sql
SELECT country,
       SUM(sales_amount) AS total_sales
FROM retail_sales
GROUP BY country;
```

Meaning:

```text
Country
   ↓
Create groups
   ↓
Calculate SUM
   ↓
Total sales for each country
```

---

## 🔹 ORDER BY

### Definition

`ORDER BY` is used to **sort the result**.

### Example

```sql
SELECT country,
       SUM(sales_amount) AS total_sales
FROM retail_sales
GROUP BY country
ORDER BY total_sales DESC;
```

`DESC`:

```text
Highest → Lowest
```

`ASC`:

```text
Lowest → Highest
```

---

## ⭐ WHERE vs HAVING

| Clause     | Works on              |
| ---------- | --------------------- |
| `WHERE`    | Individual rows       |
| `GROUP BY` | Creates groups        |
| `HAVING`   | Groups                |
| `ORDER BY` | Sorts result          |
| `LIMIT`    | Limits number of rows |

### Example

```sql
SELECT category,
       SUM(sales_amount) AS total_sales
FROM retail_sales
WHERE country = 'India'
GROUP BY category
HAVING SUM(sales_amount) > 5000
ORDER BY total_sales DESC
LIMIT 3;
```

### 🧠 Memory

```text
WHERE  → Filter Rows
GROUP BY → Make Groups
HAVING → Filter Groups
ORDER BY → Sort
LIMIT → Reduce Results
```

### 🎤 Interview Answer

> **GROUP BY creates groups based on one or more columns, while ORDER BY sorts the final result in ascending or descending order.**

---

# 2️⃣ SQL JOINs

## 🔹 What is JOIN?

A `JOIN` is used to **combine data from multiple tables using a related column**.

Example:

```text
customers
    ↓
customer_id
    ↓
orders
```

```sql
SELECT c.customer_name,
       o.payment_method
FROM customers c
JOIN orders o
ON c.customer_id = o.customer_id;
```

### Important Parts

```text
JOIN → Combine tables
ON   → Tell SQL how tables are connected
```

---

## 🔹 INNER JOIN

Returns only **matching rows**.

```sql
SELECT *
FROM customers c
INNER JOIN orders o
ON c.customer_id = o.customer_id;
```

### 🧠

```text
INNER = Matching
```

---

## 🔹 LEFT JOIN

Returns:

```text
All rows from LEFT table
+
Matching rows from RIGHT table
```

```sql
SELECT *
FROM customers c
LEFT JOIN orders o
ON c.customer_id = o.customer_id;
```

### 🧠

```text
LEFT = Keep everything from LEFT
```

---

## 🔹 RIGHT JOIN

Returns:

```text
All rows from RIGHT table
+
Matching rows from LEFT table
```

```sql
SELECT *
FROM customers c
RIGHT JOIN orders o
ON c.customer_id = o.customer_id;
```

### 🧠

```text
RIGHT = Keep everything from RIGHT
```

---

## 🔹 FULL OUTER JOIN

Returns:

```text
All rows from both tables
```

```text
Matching + Non-matching
```

### 🧠

```text
FULL = Keep BOTH
```

---

## 🔹 CROSS JOIN

Creates **every possible combination** of rows.

If:

```text
Customers = 5
Products = 4
```

then:

```text
5 × 4 = 20 combinations
```

### 🧠

```text
CROSS = Every combination
```

---

## 🔹 SELF JOIN

A table joins with **itself**.

Typical example:

```text
Employee → Manager
```

```sql
SELECT emp.emp_name AS employee,
       mng.emp_name AS manager
FROM employee emp
JOIN employee mng
ON emp.manager_id = mng.emp_id;
```

### 🧠

```text
SELF = Same table twice
```

---

## 🔹 NATURAL JOIN

Automatically joins using columns with matching names and compatible types.

### Beginner caution

It can be convenient, but explicit `JOIN ... ON ...` is generally easier to understand and control.

---

## ⭐ JOIN Summary

| JOIN    | Meaning                                  |
| ------- | ---------------------------------------- |
| INNER   | Matching rows                            |
| LEFT    | All left + matching right                |
| RIGHT   | All right + matching left                |
| FULL    | All rows from both                       |
| CROSS   | Every combination                        |
| SELF    | Same table joined with itself            |
| NATURAL | Automatically uses matching column names |

### 🎤 Interview Answer

> **JOIN is used to combine data from multiple tables using a related column. INNER JOIN returns matching rows, while LEFT, RIGHT and FULL JOIN preserve rows from their respective sides.**

---

# 3️⃣ Window Functions

## 🔹 What is a Window Function?

A window function performs calculations across related rows **without collapsing the rows into one row**.

Example:

```sql
SELECT *,
       SUM(salary) OVER(
           PARTITION BY department
       ) AS department_total
FROM employees;
```

Every employee remains in the result.

---

## 🔹 `OVER()`

`OVER()` defines the window.

```sql
SUM(salary) OVER(...)
```

---

## 🔹 `PARTITION BY`

Divides rows into groups for calculation.

```sql
SUM(salary) OVER(
    PARTITION BY department
)
```

### 🧠

```text
PARTITION BY
=
Separate groups
but
Keep original rows
```

---

## 🔹 GROUP BY vs Window Function

This is **very important**.

### GROUP BY

```sql
SELECT department,
       SUM(salary)
FROM employees
GROUP BY department;
```

Result:

```text
IT
HR
Sales
```

One row per department.

### Window Function

```sql
SELECT *,
       SUM(salary) OVER(
           PARTITION BY department
       )
FROM employees;
```

Employees remain visible.

### 🧠 Golden Rule

```text
GROUP BY
→ Reduce rows

Window Function
→ Keep rows
```

---

# 🔹 Important Window Functions

| Function         | Meaning                  |
| ---------------- | ------------------------ |
| `ROW_NUMBER()`   | Unique sequential number |
| `RANK()`         | Rank with gaps           |
| `DENSE_RANK()`   | Rank without gaps        |
| `LAG()`          | Previous row             |
| `LEAD()`         | Next row                 |
| `FIRST_VALUE()`  | First value              |
| `LAST_VALUE()`   | Last value               |
| `NTH_VALUE()`    | Nth value                |
| `NTILE()`        | Divide rows into buckets |
| `CUME_DIST()`    | Cumulative distribution  |
| `PERCENT_RANK()` | Relative rank            |

---

## Example: Ranking Employees

```sql
SELECT *,
       RANK() OVER(
           PARTITION BY department
           ORDER BY salary DESC
       ) AS salary_rank
FROM employees;
```

Meaning:

```text
For each department
       ↓
Sort salary highest to lowest
       ↓
Assign rank
```

---

## Example: Previous Salary

```sql
SELECT *,
       LAG(salary) OVER(
           PARTITION BY department
           ORDER BY salary DESC
       ) AS previous_salary
FROM employees;
```

### 🧠

```text
LAG  → Previous
LEAD → Next
```

---

# 4️⃣ Window Functions + Subquery

This is an important combination.

## 🔹 Why use a Subquery with Window Functions?

Sometimes we calculate a window-function result first and then need to **filter that result**.

For example:

> Find the second employee in each department based on salary.

### Step 1 — Calculate row number

```sql
SELECT *,
       ROW_NUMBER() OVER(
           PARTITION BY department
           ORDER BY salary DESC
       ) AS rn
FROM employees;
```

Now we have:

```text
Employee
Salary
rn
```

### Step 2 — Filter `rn = 2`

We put the first query inside a subquery:

```sql
SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER(
               PARTITION BY department
               ORDER BY salary DESC
           ) AS rn
    FROM employees
) x
WHERE rn = 2;
```

### 🧠 Flow

```text
employees
    ↓
ROW_NUMBER()
    ↓
Subquery
    ↓
WHERE rn = 2
    ↓
Second employee
```

---

## 🔥 Why not directly use `WHERE rn = 2`?

Because `rn` is created by the window function in the `SELECT` stage, so we use an outer query/subquery to filter the calculated result.

### 🧠 Remember

```text
Calculate first
     ↓
Filter later
```

---

# 5️⃣ CTE — Common Table Expression

## 🔹 Definition

A CTE is a **temporary named result set** created using `WITH`.

### Syntax

```sql
WITH cte_name AS (
    SELECT ...
)
SELECT *
FROM cte_name;
```

---

## Example

```sql
WITH electronics_orders AS (
    SELECT *
    FROM orders
    WHERE category = 'Electronics'
)
SELECT *
FROM electronics_orders;
```

### Flow

```text
orders
   ↓
Filter Electronics
   ↓
electronics_orders
   ↓
SELECT
```

---

# 🔹 Why use CTE?

CTEs help us:

* Break complex queries into steps
* Improve readability
* Organize SQL logic
* Use intermediate results
* Combine multiple processing steps

### 🧠

> **CTE = Give a name to an intermediate query result.**

---

# 🔹 Multiple CTEs

You can create multiple CTEs:

```sql
WITH cust_sales AS (
    SELECT customer_id,
           SUM(amount) AS total_sales
    FROM orders
    GROUP BY customer_id
),
cust_rank AS (
    SELECT customer_id,
           total_sales,
           RANK() OVER(
               ORDER BY total_sales DESC
           ) AS sales_rank
    FROM cust_sales
)
SELECT *
FROM cust_rank;
```

### Flow

```text
orders
   ↓
cust_sales
   ↓
cust_rank
   ↓
Final SELECT
```

---

## 🔥 CTE + JOIN

You can also join a CTE with another table.

```sql
WITH cust_sales AS (
    SELECT customer_id,
           SUM(amount) AS total_sales
    FROM orders
    GROUP BY customer_id
)
SELECT c.customer_name,
       cs.total_sales
FROM customers c
JOIN cust_sales cs
ON c.customer_id = cs.customer_id;
```

---

## CTE vs Subquery

| CTE                       | Subquery                                     |
| ------------------------- | -------------------------------------------- |
| Uses `WITH`               | Uses `(SELECT...)`                           |
| Has a name                | Usually nested directly                      |
| Good for multi-step logic | Good for simple nested logic                 |
| Multiple CTEs possible    | Queries can be nested                        |
| Improves readability      | Can become harder to read when deeply nested |

### 🧠

```text
Simple → Subquery
Multi-step → CTE
```

This is a useful beginner rule, although both techniques can often solve the same problem.

---

# 6️⃣ SQL Procedures

## 🔹 Definition

A Procedure is a **stored database program** used to perform database operations.

PostgreSQL syntax:

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

---

# 🔹 Example: Update Employee Salary

```sql
CREATE OR REPLACE PROCEDURE update_employee_salary(
    p_emp_id INT,
    p_salary INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE employee
    SET salary = p_salary
    WHERE emp_id = p_emp_id;
END;
$$;
```

Call it:

```sql
CALL update_employee_salary(103, 6000);
```

Meaning:

```text
103
 ↓
Employee ID

6000
 ↓
New Salary

UPDATE employee
 ↓
Salary = 6000
```

---

# 🔹 Procedure Parameters

```sql
p_emp_id INT,
p_salary INT
```

These are input parameters.

When we call:

```sql
CALL update_employee_salary(103, 6000);
```

the values are:

```text
p_emp_id = 103
p_salary = 6000
```

---

# 🔹 Procedure vs Function

Very important interview question.

| Procedure                                | Function                                              |
| ---------------------------------------- | ----------------------------------------------------- |
| `CREATE PROCEDURE`                       | `CREATE FUNCTION`                                     |
| `CALL`                                   | Commonly `SELECT`                                     |
| Used to perform operations               | Designed to return a result                           |
| No normal `RETURNS` clause               | Can use `RETURNS`                                     |
| Can perform `UPDATE`, `INSERT`, `DELETE` | Can also perform database logic depending on function |
| Example: Update salary                   | Example: Return IT employees                          |

### 🧠 Golden Trick

> **Procedure → DO something**
> **Function → RETURN something**

---

# 🏆 ALL TOPICS — ONE BIG SUMMARY

|  # | Topic             | Main Purpose                       | Golden Keyword         |
| -: | ----------------- | ---------------------------------- | ---------------------- |
|  1 | `GROUP BY`        | Create groups                      | **GROUP**              |
|  2 | `ORDER BY`        | Sort results                       | **SORT**               |
|  3 | JOIN              | Combine tables                     | **COMBINE**            |
|  4 | Window Function   | Calculate while keeping rows       | **KEEP ROWS**          |
|  5 | Window + Subquery | Calculate then filter              | **CALCULATE → FILTER** |
|  6 | CTE               | Organize multi-step queries        | **WITH**               |
|  7 | Procedure         | Perform stored database operations | **CALL**               |

---

# 🧠 SUPER EASY MEMORY MAP

```text
                 SQL
                  │
       ┌──────────┼──────────┐
       │          │          │
    GROUP BY    JOIN      WINDOW
       │          │          │
     GROUP      COMBINE    CALCULATE
                           KEEP ROWS
                              │
                         ┌────┴────┐
                         │         │
                     Subquery     CTE
                         │         │
                  Calculate →   WITH
                    Filter
                             
                         PROCEDURE
                             │
                            CALL
                             │
                         DO ACTION
```

---

# 🎯 Business Question → SQL Concept

| If interviewer asks...                          | Think             |
| ----------------------------------------------- | ----------------- |
| Country-wise sales                              | `GROUP BY`        |
| Sort highest sales                              | `ORDER BY DESC`   |
| Only groups above a total                       | `HAVING`          |
| Customer + orders                               | `JOIN`            |
| All customers including those without orders    | `LEFT JOIN`       |
| Employee + manager                              | `SELF JOIN`       |
| Department salary total while showing employees | Window Function   |
| Employee ranking                                | `RANK()`          |
| Unique row numbering                            | `ROW_NUMBER()`    |
| Previous employee                               | `LAG()`           |
| Next employee                                   | `LEAD()`          |
| First value                                     | `FIRST_VALUE()`   |
| Last value                                      | `LAST_VALUE()`    |
| Second value                                    | `NTH_VALUE()`     |
| Divide into groups                              | `NTILE()`         |
| Find second employee per department             | Window + Subquery |
| Break complex query into steps                  | CTE               |
| Multiple query steps                            | Multiple CTEs     |
| Update employee salary through stored logic     | Procedure         |
| Execute procedure                               | `CALL`            |

---

# 🔥 SQL Query Order — MUST REMEMBER

When **writing** a normal query:

```text
SELECT
FROM
JOIN
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```

### Example

```sql
SELECT category,
       SUM(amount) AS total_sales
FROM orders
WHERE amount > 1000
GROUP BY category
HAVING SUM(amount) > 5000
ORDER BY total_sales DESC
LIMIT 3;
```

---

# ⭐ Logical Execution Order

For understanding how SQL processes a query:

```text
FROM
  ↓
JOIN / ON
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

### 🧠 Beginner Trick

> **GET → FILTER → GROUP → FILTER GROUP → SELECT → SORT → LIMIT**

---

# 🏆 Final Interview Cheat Sheet

```text
GROUP BY
→ Make Groups

ORDER BY
→ Sort

WHERE
→ Filter Rows

HAVING
→ Filter Groups

JOIN
→ Combine Tables

ON
→ Connect Tables

INNER JOIN
→ Matching

LEFT JOIN
→ Keep Left

RIGHT JOIN
→ Keep Right

FULL JOIN
→ Keep Both

SELF JOIN
→ Same Table

WINDOW FUNCTION
→ Calculate + Keep Rows

PARTITION BY
→ Separate Window Groups

ROW_NUMBER
→ Unique Number

RANK
→ Rank + Gaps

DENSE_RANK
→ Rank + No Gaps

LAG
→ Previous

LEAD
→ Next

FIRST_VALUE
→ First

LAST_VALUE
→ Last

NTH_VALUE
→ Nth

NTILE
→ Buckets

CUME_DIST
→ Cumulative Distribution

PERCENT_RANK
→ Relative Rank

SUBQUERY
→ Query Inside Query

CTE
→ Named Temporary Query Result

WITH
→ Create CTE

PROCEDURE
→ Perform Action

CALL
→ Execute Procedure

FUNCTION
→ Return Result
```

## 🎤 If the interviewer asks: "Explain the SQL concepts you know."

A beginner-friendly answer you can remember:

> **"I have learned SQL grouping and sorting using GROUP BY and ORDER BY, joining multiple tables using different JOINs, window functions for calculations and ranking without reducing rows, subqueries for using one query's result inside another query, CTEs for organizing complex multi-step queries, and PostgreSQL procedures for storing and executing database operations."**

**This is your core PostgreSQL revision sheet.** For the mock interview, focus especially on the **bold concepts and the one-line memory rules** rather than trying to memorize every query.
