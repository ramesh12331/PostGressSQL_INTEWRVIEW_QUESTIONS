# 📘 SQL Procedures — Beginner Mock Interview

Based on your **SQL – Procedures** notes, here is a beginner-friendly mock interview revision. I’ll also clearly point out the important **PostgreSQL Procedure vs Function** difference because your notes demonstrate both.

---

# 1. What is a Procedure?

**Answer:**
A procedure is a stored database program that contains SQL statements and can be executed when needed.

In PostgreSQL, a procedure is created using:

```sql
CREATE OR REPLACE PROCEDURE
```

and executed using:

```sql
CALL
```

### 🧠 Easy meaning

> **Procedure = Save SQL logic in the database and execute it when required.**

---

# 2. Why do we use Procedures?

Procedures are useful when we want to store and execute database operations such as:

* `INSERT`
* `UPDATE`
* `DELETE`
* Business logic
* Multiple SQL statements

For example, your notes use a procedure to **update an employee's salary**.

---

# 3. What is the basic syntax of a PostgreSQL Procedure?

```sql
CREATE OR REPLACE PROCEDURE procedure_name()
LANGUAGE plpgsql
AS $$
BEGIN

    -- SQL statements

END;
$$;
```

To execute:

```sql
CALL procedure_name();
```

### 🧠 Remember

```text
CREATE PROCEDURE
       ↓
LANGUAGE plpgsql
       ↓
BEGIN
       ↓
SQL statements
       ↓
END
       ↓
CALL
```

---

# 4. What is `PL/pgSQL`?

**Answer:**
`PL/pgSQL` is PostgreSQL's procedural language used to write database functions and procedures.

Example:

```sql
LANGUAGE plpgsql
```

### 🧠 Interview answer

> **PL/pgSQL is PostgreSQL's procedural language that allows us to write procedural logic along with SQL statements.**

---

# 5. What is `BEGIN` and `END`?

Inside a PL/pgSQL procedure:

```sql
BEGIN

    -- statements

END;
```

`BEGIN` marks the start of the procedure's procedural block, and `END` marks its end.

### 🧠 Trick

```text
BEGIN → Start
END   → Finish
```

---

# 6. Explain your `hr_department` Procedure

Your notes contain:

```sql
CREATE OR REPLACE PROCEDURE hr_department()
LANGUAGE plpgsql
AS $$
BEGIN
    SELECT *
    FROM employee
    WHERE dept_name = 'HR';
END;
$$;
```

The intention is:

```text
employee table
      ↓
dept_name = 'HR'
      ↓
HR employees
```

However, there is an **important PostgreSQL point** here.

A PostgreSQL procedure does **not return a query result set in the same way a table-returning function does**. So this procedure is not a correct way to return rows for:

```sql
SELECT * FROM hr_department();
```

For returning rows, your notes' next approach—using a **function with `RETURNS TABLE`**—is the appropriate pattern.

---

# 7. Procedure vs Function — Very Important ⭐

This is one of the most important questions from your notes.

| Procedure                              | Function                                    |
| -------------------------------------- | ------------------------------------------- |
| Created with `CREATE PROCEDURE`        | Created with `CREATE FUNCTION`              |
| Called using `CALL`                    | Usually invoked using `SELECT`              |
| Does not use a normal `RETURNS` clause | Can have `RETURNS`                          |
| Commonly used for actions/operations   | Commonly used when a value/result is needed |
| Example: update salary                 | Example: return IT employees                |

### 🧠 Golden Trick

```text
PROCEDURE → DO something
FUNCTION  → RETURN something
```

---

# 8. Why did your notes use a Function for `it_department`?

Your notes define:

```sql
CREATE OR REPLACE FUNCTION it_department()
RETURNS TABLE (
    emp_id INT,
    emp_name VARCHAR(100),
    dept_name VARCHAR(100),
    salary INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN QUERY
    SELECT e.emp_id,
           e.emp_name,
           e.dept_name,
           e.salary
    FROM employee e
    WHERE e.dept_name = 'IT';
END;
$$;
```

This function is designed to **return multiple employee rows**.

That's why it has:

```sql
RETURNS TABLE (...)
```

and:

```sql
RETURN QUERY
```

Then you can use:

```sql
SELECT *
FROM it_department();
```

---

# 9. What does `RETURNS TABLE` mean?

```sql
RETURNS TABLE (
    emp_id INT,
    emp_name VARCHAR(100),
    dept_name VARCHAR(100),
    salary INT
)
```

It tells PostgreSQL that the function returns a table-like result with these columns.

### 🧠 Simple meaning

> `RETURNS TABLE` → **This function returns multiple rows and columns.**

---

# 10. What is `RETURN QUERY`?

Inside a PL/pgSQL function:

```sql
RETURN QUERY
SELECT ...
```

means the result of that query is returned by the function.

Example:

```sql
RETURN QUERY
SELECT e.emp_id,
       e.emp_name,
       e.dept_name,
       e.salary
FROM employee e
WHERE e.dept_name = 'IT';
```

### 🧠 Trick

```text
RETURN QUERY
     ↓
Return SELECT result
```

---

# 11. How do you call a Procedure?

Use:

```sql
CALL procedure_name();
```

For example:

```sql
CALL update_employee_salary(103, 6000);
```

**Important:** Procedures use `CALL`, not `SELECT * FROM procedure_name()`.

---

# 12. Explain `update_employee_salary`

Your procedure:

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

It accepts two parameters:

```text
p_emp_id
p_salary
```

Then:

```sql
UPDATE employee
SET salary = p_salary
WHERE emp_id = p_emp_id;
```

updates the matching employee's salary.

---

# 13. What are Procedure Parameters?

Parameters are values passed into the procedure when calling it.

Example:

```sql
CALL update_employee_salary(103, 6000);
```

Here:

```text
103  → p_emp_id
6000 → p_salary
```

So PostgreSQL performs:

```sql
UPDATE employee
SET salary = 6000
WHERE emp_id = 103;
```

### 🧠 Trick

> **Parameter = Input value given to the procedure.**

---

# 14. What does `p_emp_id` mean?

`p_emp_id` is simply a parameter name chosen by the developer.

The `p_` prefix is a common naming convention meaning:

```text
p_ = parameter
```

For example:

```text
p_emp_id
p_salary
p_name
p_amount
```

It is a naming convention, not a special PostgreSQL keyword.

---

# 15. What happens when you run this?

```sql
CALL update_employee_salary(103, 6000);
```

Before:

```text
emp_id = 103
salary = 4000
```

After:

```text
emp_id = 103
salary = 6000
```

You can verify:

```sql
SELECT *
FROM employee
WHERE emp_id = 103;
```

### Flow

```text
CALL
 ↓
Procedure receives 103, 6000
 ↓
Find emp_id = 103
 ↓
Update salary
 ↓
6000
```

---

# 16. What does `CREATE OR REPLACE PROCEDURE` mean?

```sql
CREATE OR REPLACE PROCEDURE
```

means:

> Create the procedure if it doesn't exist, or replace its definition if it already exists.

This is useful while developing because you can modify the procedure and run the statement again.

---

# 17. What is `DROP PROCEDURE`?

Your notes contain:

```sql
DROP PROCEDURE IF EXISTS it_department();
```

The purpose of `DROP PROCEDURE` is to remove an existing procedure.

### General syntax

```sql
DROP PROCEDURE IF EXISTS procedure_name(parameters);
```

### `IF EXISTS`

Prevents an error if the procedure doesn't exist.

---

# 18. Why does PostgreSQL use `$$`?

Example:

```sql
AS $$
BEGIN
    ...
END;
$$;
```

`$$` is **dollar quoting**.

It allows PostgreSQL to treat the contents between the two `$$` markers as the procedure/function body.

### 🧠 Simple memory

```text
$$
  Procedure code
$$
```

Think:

> **`$$` = Start and end of the procedure body**

---

# 19. Can a Procedure perform UPDATE?

**Yes.**

Your example:

```sql
UPDATE employee
SET salary = p_salary
WHERE emp_id = p_emp_id;
```

This is a typical use of a procedure.

---

# 20. Procedure vs Normal SQL Query

### Normal SQL

```sql
UPDATE employee
SET salary = 6000
WHERE emp_id = 103;
```

You write the SQL directly each time.

### Procedure

```sql
CALL update_employee_salary(103, 6000);
```

The SQL logic is already stored in the database.

### 🧠 Real-time idea

Instead of repeatedly writing:

```sql
UPDATE ...
```

you can call:

```sql
CALL update_employee_salary(...);
```

---

# 🔥 Procedure vs Function — Interview Table

| Question        | Procedure                             | Function                      |
| --------------- | ------------------------------------- | ----------------------------- |
| Create          | `CREATE PROCEDURE`                    | `CREATE FUNCTION`             |
| Execute         | `CALL`                                | `SELECT` / expression context |
| Return value    | Not through a normal `RETURNS` clause | Yes                           |
| Return table    | Not like a table-returning function   | `RETURNS TABLE`               |
| Return query    | ❌ Not in the same way                 | `RETURN QUERY`                |
| Typical purpose | Perform an operation                  | Calculate/return a result     |
| Your example    | Update salary                         | Return IT employees           |

---

# 🎤 25 SQL Procedures Mock Interview Questions

|  # | Interview Question                                        | Short Answer                                                    |
| -: | --------------------------------------------------------- | --------------------------------------------------------------- |
|  1 | What is a procedure?                                      | A stored database program used to execute SQL/procedural logic. |
|  2 | How do you create a procedure in PostgreSQL?              | Using `CREATE OR REPLACE PROCEDURE`.                            |
|  3 | How do you execute a procedure?                           | Using `CALL`.                                                   |
|  4 | What language is used in your procedure?                  | `PL/pgSQL`.                                                     |
|  5 | What is `BEGIN`?                                          | Starts the procedural block.                                    |
|  6 | What is `END`?                                            | Ends the procedural block.                                      |
|  7 | What are parameters?                                      | Values passed to a procedure/function.                          |
|  8 | What does `p_` usually mean?                              | A naming convention for a parameter.                            |
|  9 | Can a procedure perform UPDATE?                           | Yes.                                                            |
| 10 | Can a procedure perform INSERT?                           | Yes.                                                            |
| 11 | Can a procedure perform DELETE?                           | Yes.                                                            |
| 12 | What does `CREATE OR REPLACE` do?                         | Creates or replaces the existing definition.                    |
| 13 | What does `DROP PROCEDURE` do?                            | Removes a procedure.                                            |
| 14 | What is `IF EXISTS`?                                      | Prevents an error if the object doesn't exist.                  |
| 15 | What is `$$`?                                             | PostgreSQL dollar quoting for the procedure/function body.      |
| 16 | How do you pass values to a procedure?                    | Pass them inside `CALL`.                                        |
| 17 | What does `CALL update_employee_salary(103,6000)` do?     | Updates employee 103's salary to 6000.                          |
| 18 | Can a procedure directly be queried with `SELECT * FROM`? | Not like a table-returning function.                            |
| 19 | What is a function?                                       | A stored database routine designed to return a value/result.    |
| 20 | How do you call a function?                               | Commonly using `SELECT function_name(...)`.                     |
| 21 | What is `RETURNS TABLE`?                                  | Defines a function that returns rows and columns.               |
| 22 | What is `RETURN QUERY`?                                   | Returns the result of a query from a PL/pgSQL function.         |
| 23 | Procedure vs function?                                    | Procedure performs operations; function returns a result.       |
| 24 | Why use a procedure?                                      | To store reusable database operations/business logic.           |
| 25 | What is the main memory trick?                            | **Procedure → DO; Function → RETURN.**                          |

---

# ⭐ Your Three Important Examples

## Example 1 — Procedure

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

Execute:

```sql
CALL update_employee_salary(103, 6000);
```

---

## Example 2 — Function Returning Employees

```sql
CREATE OR REPLACE FUNCTION it_department()
RETURNS TABLE (
    emp_id INT,
    emp_name VARCHAR(100),
    dept_name VARCHAR(100),
    salary INT
)
LANGUAGE plpgsql
AS $$
BEGIN
    RETURN QUERY
    SELECT e.emp_id,
           e.emp_name,
           e.dept_name,
           e.salary
    FROM employee e
    WHERE e.dept_name = 'IT';
END;
$$;
```

Execute:

```sql
SELECT *
FROM it_department();
```

---

# 🧠 Procedure Golden Memory

```text
PROCEDURE
   ↓
CREATE
   ↓
PL/pgSQL
   ↓
BEGIN
   ↓
SQL Operation
   ↓
END
   ↓
CALL
```

### Function

```text
FUNCTION
   ↓
CREATE
   ↓
RETURNS
   ↓
BEGIN
   ↓
RETURN / RETURN QUERY
   ↓
SELECT function()
```

---

# 🏆 Final Saturday Revision

| Topic                       | Golden Word                |
| --------------------------- | -------------------------- |
| Procedure creation          | `CREATE PROCEDURE`         |
| Procedure execution         | `CALL`                     |
| Procedural language         | `PL/pgSQL`                 |
| Start block                 | `BEGIN`                    |
| End block                   | `END`                      |
| Parameters                  | `p_...`                    |
| Procedure body              | `$$ ... $$`                |
| Modify data                 | `INSERT / UPDATE / DELETE` |
| Function output             | `RETURNS`                  |
| Multiple rows from function | `RETURNS TABLE`            |
| Return SELECT result        | `RETURN QUERY`             |

### 🔥 One sentence to remember for the interview:

> **A PostgreSQL procedure is a stored program used mainly to perform database operations, and we execute it using `CALL`; a function is designed to return a value or result and can be invoked with `SELECT`.**
