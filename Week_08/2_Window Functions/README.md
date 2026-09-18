# 🪟 PostgreSQL Window Functions — Beginner Mock Interview

I reviewed the Window Functions material you shared. It covers `SUM() OVER()`, `MIN()`, `MAX()`, `COUNT()`, window frames, named windows, `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`, `LAG()`, `LEAD()`, and using a subquery to filter a window-function result. 

For your mock, don't memorize the long queries first. Understand this one idea:

> **GROUP BY reduces rows into groups. Window Functions calculate across related rows but keep the original rows.**

---

# 🧠 1. What is a Window Function?

### Beginner understanding

Suppose we have:

```text
Name      Department   Salary
Ramesh    IT           60000
Suresh    IT           75000
Mahesh    IT           75000
Priya     HR           55000
Divya     HR           65000
```

If we use `GROUP BY`:

```sql
SELECT department, SUM(salary)
FROM employees
GROUP BY department;
```

We get only:

```text
IT    210000
HR    120000
```

Individual employee rows are gone.

But with a Window Function:

```sql
SELECT *,
       SUM(salary) OVER(PARTITION BY department) AS total_salary
FROM employees;
```

we can get:

```text
Ramesh   IT   60000   210000
Suresh   IT   75000   210000
Mahesh   IT   75000   210000
Priya    HR   55000   120000
Divya    HR   65000   120000
```

### 🎤 Interview Answer

> **A window function performs a calculation across a set of related rows without combining those rows into a single row.**

### 🧠 Memory

> **Window Function = Calculate + Keep original rows**

---

# 2. What is `OVER()`?

`OVER()` tells PostgreSQL:

> **"Perform this calculation over a window of rows."**

Example:

```sql
SELECT *,
       SUM(salary) OVER() AS total_salary
FROM employees;
```

### 🎤 Interview Answer

> **OVER() defines the window of rows on which a window function operates.**

### Memory

```text
OVER() → Window
```

---

# 3. What is `PARTITION BY`?

### Beginner understanding

`PARTITION BY` divides the data into separate groups **for the window calculation**.

```sql
SUM(salary) OVER(
    PARTITION BY department
)
```

Think:

```text
All employees
     ↓
 ┌─────────┬─────────┐
 ↓         ↓
 IT        HR
 ↓         ↓
Calculate Calculate
separately separately
```

### 🎤 Interview Answer

> **PARTITION BY divides rows into groups within a window function, and the calculation is performed separately for each group.**

### Memory

> **PARTITION BY = Separate groups for calculation**

---

# 4. GROUP BY vs PARTITION BY ⭐⭐⭐

This is a **very important mock question**.

| GROUP BY                    | PARTITION BY                   |
| --------------------------- | ------------------------------ |
| Creates groups              | Creates windows/groups         |
| Usually reduces rows        | Keeps original rows            |
| Used with aggregate queries | Used with window functions     |
| One result per group        | Result can appear on every row |

### 🧠 Easy example

```text
GROUP BY
IT → 210000
HR → 120000
```

But:

```text
PARTITION BY
Ramesh → IT → 210000
Suresh → IT → 210000
Priya  → HR → 120000
```

### 🎤 Interview Answer

> **GROUP BY combines rows into groups, while PARTITION BY divides rows into windows without removing the individual rows.**

---

# 5. Can aggregate functions be used as window functions?

**Yes.**

For example:

```sql
SELECT *,
       SUM(salary) OVER(PARTITION BY department) AS total_salary
FROM employees;
```

You can use:

```text
SUM()
AVG()
MIN()
MAX()
COUNT()
```

with `OVER()`.

### 🧠 Memory

```text
SUM() + OVER()   → Window SUM
AVG() + OVER()   → Window AVG
COUNT() + OVER() → Window COUNT
```

---

# 6. What is `ROW_NUMBER()`?

### Beginner understanding

It gives every row a **unique sequential number**.

```sql
SELECT *,
       ROW_NUMBER() OVER(
           PARTITION BY department
           ORDER BY salary DESC
       ) AS row_number
FROM employees;
```

For IT:

```text
75000 → 1
75000 → 2
60000 → 3
50000 → 4
```

### 🎤 Interview Answer

> **ROW_NUMBER() assigns a unique sequential number to each row within the specified window.**

### Memory

> **ROW_NUMBER = Unique number**

---

# 7. What is `RANK()`?

`RANK()` gives the **same rank to tied values**.

Suppose:

```text
75000
75000
60000
50000
```

Result:

```text
75000 → 1
75000 → 1
60000 → 3
50000 → 4
```

### 🎤 Interview Answer

> **RANK() assigns the same rank to rows with equal values, and it leaves gaps after tied ranks.**

### Memory

> **RANK = Tie + Gap**

---

# 8. What is `DENSE_RANK()`?

Suppose:

```text
75000
75000
60000
50000
```

Result:

```text
75000 → 1
75000 → 1
60000 → 2
50000 → 3
```

### 🎤 Interview Answer

> **DENSE_RANK() gives the same rank to tied rows but does not leave gaps after the tie.**

### Memory

> **DENSE_RANK = Tie + No Gap**

---

# 🔥 9. ROW_NUMBER vs RANK vs DENSE_RANK

This is **must remember**.

Suppose:

```text
Salary
75000
75000
60000
50000
```

| Salary | ROW_NUMBER | RANK | DENSE_RANK |
| -----: | ---------: | ---: | ---------: |
|  75000 |          1 |    1 |          1 |
|  75000 |          2 |    1 |          1 |
|  60000 |          3 |    3 |          2 |
|  50000 |          4 |    4 |          3 |

### 🧠 Golden Memory

```text
ROW_NUMBER → No same number
RANK       → Same rank + gaps
DENSE_RANK → Same rank + no gaps
```

---

# 10. Why do we use `ORDER BY` inside `OVER()`?

Example:

```sql
ROW_NUMBER() OVER(
    PARTITION BY department
    ORDER BY salary DESC
)
```

### 🧠 Meaning

```text
PARTITION BY department
        ↓
Separate departments
        ↓
ORDER BY salary DESC
        ↓
Highest salary gets first position
```

### 🎤 Interview Answer

> **ORDER BY inside OVER() determines the order in which the window function processes or ranks rows.**

---

# 11. What is `LAG()`?

### Beginner understanding

`LAG()` gets a value from a **previous row**.

Your material uses:

```sql
LAG(salary) OVER(
    PARTITION BY department
    ORDER BY salary DESC
)
```

### Think

```text
Current employee
       ↑
Previous employee's salary
```

### 🎤 Interview Answer

> **LAG() is used to access a value from a previous row within the window.**

### Memory

> **LAG = Previous**

---

# 12. What is `LEAD()`?

`LEAD()` gets a value from the **next row**.

```sql
LEAD(salary) OVER(
    PARTITION BY department
    ORDER BY salary DESC
)
```

### 🎤 Interview Answer

> **LEAD() is used to access a value from a following row within the window.**

### Memory

```text
LAG  → Previous
LEAD → Next
```

---

# 🔥 13. LAG vs LEAD

| LAG            | LEAD           |
| -------------- | -------------- |
| Previous row   | Next row       |
| Looks backward | Looks forward  |
| `LAG(column)`  | `LEAD(column)` |

### 🧠 Easy trick

> **LAG = Back**
> **LEAD = Forward**

---

# 14. Can we use a Window Function in WHERE?

### ❌ Not directly

For example, this approach from your notes is wrong:

```sql
SELECT *,
       ROW_NUMBER() OVER(
           PARTITION BY department
           ORDER BY salary DESC
       ) AS row_number
FROM employees
WHERE row_number = 2;
```

The reason is that the window result is not available to the `WHERE` clause at that logical stage.

Your material correctly demonstrates using an outer query instead. 

---

# 15. How do we filter a Window Function result?

Use a **subquery**.

```sql
SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER(
               PARTITION BY department
               ORDER BY salary DESC
           ) AS row_number
    FROM employees
) AS x
WHERE x.row_number = 2;
```

### 🧠 Understand the flow

```text
INNER QUERY
     ↓
Calculate ROW_NUMBER
     ↓
Create row_number column
     ↓
OUTER QUERY
     ↓
WHERE row_number = 2
```

### 🎤 Interview Answer

> **We cannot directly filter a window-function result in the same query's WHERE clause, so we can use a subquery or CTE and filter the calculated result in the outer query.**

---

# 16. How do you find the second-highest salary in each department?

### Think first:

```text
Second highest
      ↓
ROW_NUMBER
      ↓
PARTITION BY department
      ↓
ORDER BY salary DESC
      ↓
row_number = 2
```

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

### 🧠 This pattern is important

```text
Ranking
  ↓
Subquery
  ↓
Filter rank
```

---

# 17. How do you find the highest-paid employee in each department?

Use:

```text
RANK() / ROW_NUMBER()
       ↓
PARTITION BY department
       ↓
ORDER BY salary DESC
       ↓
rank = 1
```

Example:

```sql
SELECT *
FROM (
    SELECT *,
           RANK() OVER(
               PARTITION BY department
               ORDER BY salary DESC
           ) AS rnk
    FROM employees
) x
WHERE rnk = 1;
```

### Important

Using `RANK()` means **all employees tied for the highest salary can be returned**.

Using `ROW_NUMBER()` returns only one row per department, but when salaries tie, you should add a deterministic secondary ordering if you need a predictable choice.

---

# 18. What is a Window Frame?

Your notes contain:

```sql
RANGE BETWEEN
UNBOUNDED PRECEDING
AND
UNBOUNDED FOLLOWING
```

### 🧠 Beginner meaning

A **window frame defines which rows inside the window are included in the calculation for the current row**.

Think:

```text
Window
--------------------------------
| Row | Row | Current | Row |
--------------------------------
          ↑
       Current row
```

The frame tells PostgreSQL:

> Which rows around this current row should I consider?

### Important words

```text
UNBOUNDED PRECEDING → Start from beginning
CURRENT ROW         → Current row
UNBOUNDED FOLLOWING → Go until end
```

For your beginner mock, understand these meanings first rather than memorizing every frame syntax.

---

# 19. What is a named window?

Your notes use:

```sql
WINDOW w AS (
    PARTITION BY department
    ORDER BY salary
    RANGE BETWEEN UNBOUNDED PRECEDING
          AND UNBOUNDED FOLLOWING
);
```

Then:

```sql
MIN(salary) OVER w
MAX(salary) OVER w
SUM(salary) OVER w
COUNT(*) OVER w
```

### 🧠 Why?

Instead of repeating the same window definition multiple times, we give it a name:

```text
Window definition
       ↓
       w
       ↓
Reuse it
```

### 🎤 Interview Answer

> **A named window allows us to define a window specification once and reuse it for multiple window functions in the same query.**

---

# 20. Window Function vs Aggregate Function

| Aggregate Function            | Window Function                |
| ----------------------------- | ------------------------------ |
| Calculates across rows        | Calculates across related rows |
| Can reduce rows with GROUP BY | Keeps individual rows          |
| `SUM(salary)`                 | `SUM(salary) OVER(...)`        |
| Group result                  | Row + calculation              |

### 🧠 Example

Aggregate:

```sql
SELECT department, SUM(salary)
FROM employees
GROUP BY department;
```

Window:

```sql
SELECT *,
       SUM(salary) OVER(
           PARTITION BY department
       ) AS total_salary
FROM employees;
```

### Golden memory

> **GROUP BY → Collapse**
> **Window → Keep**

---

# 🎯 21. What does `SUM(salary) OVER(PARTITION BY department)` mean?

Break it down:

```text
SUM(salary)
    ↓
Calculate salary total

OVER()
    ↓
Window calculation

PARTITION BY department
    ↓
Do it separately for each department
```

### 🎤 Interview Answer

> **It calculates the total salary for each department and displays that department total on every employee row belonging to that department.**

---

# 🎯 22. What does `SUM(salary) OVER(PARTITION BY department ORDER BY salary DESC)` do?

This is slightly more advanced.

Because `ORDER BY` is inside the window, the calculation becomes **order-sensitive**, producing a running/windowed result according to the applicable frame.

### 🧠 Don't memorize the output blindly.

Remember:

```text
PARTITION BY → Which group?
ORDER BY     → In what order?
Frame        → Which rows are included?
```

That is the better interview understanding.

---

# 🔥 23. Your Product Example

Your notes use:

```sql
SELECT *,
       SUM(price) OVER(
           PARTITION BY product_category
       ) AS product_wise_revenue
FROM product;
```

### 🧠 Meaning

For every product:

> Calculate the **total price of all products in the same category**, while still displaying every product.

So:

```text
Phone
 ↓
All Phone prices added
 ↓
Same Phone total shown for Phone rows
```

Your material also contrasts this with the `GROUP BY product_category` version. 

---

# 🏆 Most Important Interview Questions

For your Saturday mock, I would **definitely prepare these**:

|  # | Question                                        | Priority |
| -: | ----------------------------------------------- | :------: |
|  1 | What is a Window Function?                      |  🔥🔥🔥  |
|  2 | What is `OVER()`?                               |  🔥🔥🔥  |
|  3 | What is `PARTITION BY`?                         |  🔥🔥🔥  |
|  4 | GROUP BY vs PARTITION BY?                       |  🔥🔥🔥  |
|  5 | What is `ROW_NUMBER()`?                         |  🔥🔥🔥  |
|  6 | What is `RANK()`?                               |  🔥🔥🔥  |
|  7 | What is `DENSE_RANK()`?                         |  🔥🔥🔥  |
|  8 | ROW_NUMBER vs RANK?                             |  🔥🔥🔥  |
|  9 | RANK vs DENSE_RANK?                             |  🔥🔥🔥  |
| 10 | Why use ORDER BY inside OVER()?                 |   🔥🔥   |
| 11 | What is LAG()?                                  |   🔥🔥   |
| 12 | What is LEAD()?                                 |   🔥🔥   |
| 13 | LAG vs LEAD?                                    |   🔥🔥   |
| 14 | Can window functions be used directly in WHERE? |  🔥🔥🔥  |
| 15 | How do you filter a window-function result?     |  🔥🔥🔥  |
| 16 | Find second-highest salary per department       |  🔥🔥🔥  |
| 17 | Find highest salary per department              |  🔥🔥🔥  |
| 18 | What is a window frame?                         |   🔥🔥   |
| 19 | What is a named window?                         |    🔥    |
| 20 | Window Function vs Aggregate Function?          |  🔥🔥🔥  |

---

# 🧠 Final Window Function Cheat Sheet

| Concept                    | Simple Meaning                       |
| -------------------------- | ------------------------------------ |
| `OVER()`                   | Define window                        |
| `PARTITION BY`             | Separate calculation groups          |
| `ORDER BY` inside `OVER()` | Arrange rows within the window       |
| `ROW_NUMBER()`             | Unique row number                    |
| `RANK()`                   | Rank with gaps                       |
| `DENSE_RANK()`             | Rank without gaps                    |
| `LAG()`                    | Previous row                         |
| `LEAD()`                   | Next row                             |
| `SUM() OVER()`             | Window total                         |
| `AVG() OVER()`             | Window average                       |
| `MIN() OVER()`             | Window minimum                       |
| `MAX() OVER()`             | Window maximum                       |
| `COUNT() OVER()`           | Window count                         |
| Window frame               | Defines rows included in calculation |
| Named window               | Reusable window definition           |

# 🚀 The 5 Things You MUST Remember

```text
1️⃣ OVER()
   → Window Function works here

2️⃣ PARTITION BY
   → Separate groups

3️⃣ ORDER BY
   → Decide row order

4️⃣ ROW_NUMBER / RANK / DENSE_RANK
   → Ranking

5️⃣ Subquery / CTE
   → Filter the window result
```

### 🏆 Golden Formula

```sql
FUNCTION() OVER(
    PARTITION BY group_column
    ORDER BY sort_column
)
```

Read it as:

> **"For each group, arrange the rows in this order, and perform this calculation."**

That one sentence is the key to understanding most of your Window Function questions.
