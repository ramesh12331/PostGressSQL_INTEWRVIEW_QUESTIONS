# 📘 SQL CTE (Common Table Expressions) — Beginner Mock Interview

Based on your **20-08-2026 SQL – CTE** notes, here is a beginner-friendly **mock interview revision** in the same style as the previous Window Functions + Subquery topic.

---

# 1. What is a CTE?

**Answer:**
CTE stands for **Common Table Expression**.

A CTE is a **temporary named result set** that we create using the `WITH` keyword and then use in the main query.

### Simple idea

```text
WITH
  ↓
Create temporary result
  ↓
Give it a name
  ↓
Use that name in SELECT
```

### Example

```sql
WITH electronics_orders AS (
    SELECT *
    FROM orders
    WHERE category = 'Electronics'
)
SELECT *
FROM electronics_orders;
```

### 🧠 Interview Answer

> **A CTE is a temporary named result set created using the `WITH` clause. It makes complex SQL queries easier to read and organize.**

---

# 2. What is the syntax of a CTE?

```sql
WITH cte_name AS (
    -- CTE query
)
SELECT *
FROM cte_name;
```

### Remember

```text
WITH → CTE name → AS → (query) → Main query
```

---

# 3. Why do we use CTE?

CTEs are useful for:

* Breaking a complex query into smaller steps
* Improving readability
* Reusing the result in the main query
* Combining multiple query steps
* Working with aggregates and window functions

### 🧠 Simple Trick

> **CTE = Give a name to an intermediate query result.**

---

# 4. Explain the `electronics_orders` CTE.

Your example:

```sql
WITH electronics_orders AS (
    SELECT *
    FROM orders
    WHERE category = 'Electronics'
)
SELECT *
FROM electronics_orders;
```

### Step-by-step

First, CTE:

```sql
SELECT *
FROM orders
WHERE category = 'Electronics';
```

This gets only Electronics orders.

Then:

```sql
SELECT *
FROM electronics_orders;
```

gets the CTE result.

### Flow

```text
orders
  ↓
category = Electronics
  ↓
electronics_orders
  ↓
SELECT *
```

---

# 5. Can we apply another filter to a CTE?

**Yes.**

Example:

```sql
WITH electronics_orders AS (
    SELECT *
    FROM orders
    WHERE category = 'Electronics'
)
SELECT *
FROM electronics_orders
WHERE product = 'Laptop';
```

Here:

### Step 1

```text
category = Electronics
```

### Step 2

```text
product = Laptop
```

So the CTE allows us to separate the filtering steps.

---

# 6. Can we use aggregate functions inside a CTE?

**Yes.**

Example from your notes:

```sql
WITH electronics_orders AS (
    SELECT *
    FROM orders
    WHERE category = 'Electronics'
)
SELECT product,
       SUM(quantity)
FROM electronics_orders
GROUP BY product;
```

### Flow

```text
orders
   ↓
Electronics only
   ↓
electronics_orders
   ↓
GROUP BY product
   ↓
SUM(quantity)
```

---

# 7. Can we use `WHERE` inside a CTE?

**Yes.**

Example:

```sql
WITH high_prods AS (
    SELECT *
    FROM orders
    WHERE amount > 20000
)
SELECT *
FROM high_prods;
```

The CTE contains only orders where:

```text
amount > 20000
```

---

# 8. Can we use `GROUP BY` inside a CTE?

**Yes.**

Example:

```sql
WITH cust_sales AS (
    SELECT customer_id,
           SUM(amount) AS total_sales
    FROM orders
    GROUP BY customer_id
)
SELECT *
FROM cust_sales
WHERE total_sales > 50000;
```

### What happens?

First:

```text
GROUP BY customer_id
```

Then:

```text
SUM(amount)
```

Then the outer query:

```text
total_sales > 50000
```

filters the customer groups.

---

# 9. Why is this better than writing everything in one query?

Instead of putting everything into one large query:

```text
Calculate customer sales
        ↓
Filter sales
        ↓
Rank customers
        ↓
Join customers
        ↓
Display result
```

we can divide it into logical CTE steps.

```text
CTE 1 → Customer Sales
          ↓
CTE 2 → Customer Rank
          ↓
Main Query → Customer Details
```

This is one of the **most important practical uses of CTEs**.

---

# 10. What is a multiple CTE?

We can define **more than one CTE** using commas.

Your example:

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
           ) AS sales_rnk
    FROM cust_sales
)
SELECT *
FROM cust_rank;
```

There are two CTEs:

```text
cust_sales
    ↓
cust_rank
    ↓
SELECT
```

---

# 11. Explain the `cust_sales` CTE.

```sql
WITH cust_sales AS (
    SELECT customer_id,
           SUM(amount) AS total_sales
    FROM orders
    GROUP BY customer_id
)
```

It calculates:

```text
Customer
   ↓
Total Sales
```

For example:

```text
customer_id | total_sales
------------|------------
101         | ...
102         | ...
103         | ...
```

---

# 12. Explain the `cust_rank` CTE.

```sql
cust_rank AS (
    SELECT customer_id,
           total_sales,
           RANK() OVER(
               ORDER BY total_sales DESC
           ) AS sales_rnk
    FROM cust_sales
)
```

It takes the result of `cust_sales` and ranks customers based on total sales.

```text
Highest sales → Rank 1
Next          → Rank 2
Next          → Rank 3
```

### Important

The second CTE uses the **first CTE**:

```text
cust_sales
     ↓
cust_rank
```

---

# 13. Can a CTE use another CTE?

**Yes.**

Example:

```sql
WITH cte1 AS (
    SELECT ...
),
cte2 AS (
    SELECT *
    FROM cte1
)
SELECT *
FROM cte2;
```

### 🧠 Trick

> **CTE can be chained.**

```text
CTE 1 → CTE 2 → CTE 3 → Main Query
```

---

# 14. Can we use a Window Function inside a CTE?

**Yes.**

Your example uses:

```sql
RANK() OVER(
    ORDER BY total_sales DESC
)
```

inside `cust_rank`.

Complete pattern:

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
           ) AS sales_rnk
    FROM cust_sales
)
SELECT *
FROM cust_rank;
```

This is a very useful combination:

```text
GROUP BY
   ↓
CTE
   ↓
Window Function
```

---

# 15. Can we JOIN a CTE with another table?

**Yes.**

Your final example:

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
           ) AS sales_rnk
    FROM cust_sales
)
SELECT c.customer_name,
       cr.total_sales,
       cr.sales_rnk
FROM customers c
JOIN cust_rank cr
    ON c.customer_id = cr.customer_id
ORDER BY cr.sales_rnk;
```

### Flow

```text
orders
   ↓
cust_sales
   ↓
cust_rank
   ↓
JOIN customers
   ↓
Final result
```

---

# 16. Explain the complete CTE example in interview style.

If interviewer asks:

> **Explain your CTE query.**

You can say:

> First, I create a `cust_sales` CTE to calculate total sales for each customer using `SUM()` and `GROUP BY`. Then I create a second CTE called `cust_rank` to rank customers based on total sales using `RANK()`. Finally, I join the ranked result with the `customers` table to display the customer name, total sales, and sales rank.

⭐ This is a **good interview answer** for your current level.

---

# 17. CTE vs Subquery

Very important interview question.

| CTE                                 | Subquery                                        |
| ----------------------------------- | ----------------------------------------------- |
| Uses `WITH`                         | Uses `(SELECT...)`                              |
| Gives a name to intermediate result | Usually directly embedded                       |
| Easier for multi-step queries       | Good for simple nested logic                    |
| Can define multiple CTEs            | Can have nested subqueries                      |
| Can chain CTEs                      | Can nest queries                                |
| Good for readability                | Can become difficult to read when deeply nested |

### Example Subquery

```sql
SELECT *
FROM orders
WHERE amount > (
    SELECT AVG(amount)
    FROM orders
);
```

### Example CTE

```sql
WITH avg_amount AS (
    SELECT AVG(amount) AS average
    FROM orders
)
SELECT *
FROM orders
WHERE amount > (
    SELECT average
    FROM avg_amount
);
```

For a simple calculation, a subquery can be shorter. For **multiple logical steps**, a CTE can make the query easier to follow.

---

# 18. CTE vs Temporary Table

| CTE                               | Temporary Table                                                  |
| --------------------------------- | ---------------------------------------------------------------- |
| Defined with `WITH`               | Created using `CREATE TEMP TABLE`                                |
| Used within the statement         | Can exist for the session                                        |
| No separate table creation needed | Creates a temporary table                                        |
| Good for query organization       | Useful when intermediate data needs to persist across statements |

### 🧠 Trick

```text
CTE
↓
Temporary result for a query

TEMP TABLE
↓
Temporary table for session/work
```

---

# 19. Is a CTE a permanent table?

**No.**

A CTE is not a permanent table.

It exists for the duration of the SQL statement in which it is defined.

### Interview Answer

> **No. A CTE is a temporary named result set available to the statement that defines it.**

---

# 20. What keyword is used to create a CTE?

**`WITH`**

Example:

```sql
WITH my_cte AS (
    SELECT *
    FROM orders
)
SELECT *
FROM my_cte;
```

---

# 21. Can we use `ORDER BY` in the final query after CTE?

**Yes.**

Your example:

```sql
SELECT c.customer_name,
       cr.total_sales,
       cr.sales_rnk
FROM customers c
JOIN cust_rank cr
    ON c.customer_id = cr.customer_id
ORDER BY cr.sales_rnk;
```

The CTE prepares the data, and the final query sorts it.

---

# 22. What is a real-time use case for CTE?

Suppose a company wants:

> **Customer name + total sales + customer rank**

We can divide the task:

```text
Step 1
Calculate total sales
       ↓
Step 2
Rank customers
       ↓
Step 3
Join customer names
       ↓
Final report
```

This is a practical reporting use case for CTEs.

---

# 🔥 CTE + Important SQL Concepts

| Requirement               | Technique            |
| ------------------------- | -------------------- |
| Filter Electronics        | `WHERE` inside CTE   |
| Amount > 20,000           | `WHERE`              |
| Customer total sales      | `SUM()` + `GROUP BY` |
| Sales > 50,000            | Outer `WHERE`        |
| Rank customers            | `RANK() OVER()`      |
| Customer details          | `JOIN`               |
| Multiple processing steps | Multiple CTEs        |
| Reuse previous result     | CTE                  |
| Organize complex query    | CTE                  |

---

# 🎤 25 CTE Mock Interview Questions

|  # | Interview Question                     | Short Answer                                                        |
| -: | -------------------------------------- | ------------------------------------------------------------------- |
|  1 | What is CTE?                           | A temporary named result set created using `WITH`.                  |
|  2 | What does CTE stand for?               | Common Table Expression.                                            |
|  3 | Which keyword creates a CTE?           | `WITH`.                                                             |
|  4 | What is the basic syntax?              | `WITH name AS (query) SELECT ...`.                                  |
|  5 | Why use CTE?                           | To make complex queries easier to read and organize.                |
|  6 | Is CTE a permanent table?              | No.                                                                 |
|  7 | Can CTE contain `WHERE`?               | Yes.                                                                |
|  8 | Can CTE contain `GROUP BY`?            | Yes.                                                                |
|  9 | Can CTE contain aggregate functions?   | Yes.                                                                |
| 10 | Can CTE contain joins?                 | Yes.                                                                |
| 11 | Can CTE contain window functions?      | Yes.                                                                |
| 12 | Can we have multiple CTEs?             | Yes.                                                                |
| 13 | How do multiple CTEs work?             | Separate them with commas.                                          |
| 14 | Can one CTE use another CTE?           | Yes.                                                                |
| 15 | Can we join a CTE with a table?        | Yes.                                                                |
| 16 | CTE vs subquery?                       | CTE is named using `WITH`; subquery is nested inside another query. |
| 17 | What does `cust_sales` calculate?      | Total sales per customer.                                           |
| 18 | Which function calculates total sales? | `SUM()`.                                                            |
| 19 | Which clause groups customer sales?    | `GROUP BY customer_id`.                                             |
| 20 | What does `cust_rank` calculate?       | Customer sales rank.                                                |
| 21 | Which function ranks customers?        | `RANK()`.                                                           |
| 22 | Why use `ORDER BY total_sales DESC`?   | Highest sales get the highest priority/rank.                        |
| 23 | Can a CTE be chained?                  | Yes.                                                                |
| 24 | Can the final query join a CTE?        | Yes.                                                                |
| 25 | What is the main benefit of CTE?       | Better readability and easier multi-step SQL.                       |

---

# 🧠 CTE Golden Memory

```text
WITH
 ↓
CTE NAME
 ↓
AS
 ↓
( QUERY )
 ↓
MAIN QUERY
```

### Multiple CTE

```text
WITH
   CTE 1
      ↓
   CTE 2
      ↓
   CTE 3
      ↓
   MAIN QUERY
```

---

# ⭐ Your CTE Example — Remember This Pattern

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
           ) AS sales_rnk
    FROM cust_sales
)
SELECT c.customer_name,
       cr.total_sales,
       cr.sales_rnk
FROM customers c
JOIN cust_rank cr
    ON c.customer_id = cr.customer_id
ORDER BY cr.sales_rnk;
```

### Read it like a story:

```text
orders
  ↓
Calculate customer sales
  ↓
cust_sales
  ↓
Rank customer sales
  ↓
cust_rank
  ↓
Join customer names
  ↓
Final report
```

## 🏆 Final 10-Second Revision

> **CTE = temporary named query result.**
> **`WITH` = creates CTE.**
> **Multiple CTEs = comma separated.**
> **CTE can contain WHERE, JOIN, GROUP BY, aggregates and window functions.**
> **One CTE can use another CTE.**
> **Final query can JOIN the CTE.**
> **Main purpose = make complex, multi-step SQL easier to read and organize.**

### 🔥 One formula to remember

```text
WITH CTE
   ↓
Prepare Data
   ↓
Transform Data
   ↓
Final SELECT
```

This topic connects directly with your previous topics:

```text
GROUP BY
   ↓
CTE
   ↓
Window Function
   ↓
JOIN
   ↓
Final Result
```

That combination is particularly important for your **PostgreSQL mock interview**.
