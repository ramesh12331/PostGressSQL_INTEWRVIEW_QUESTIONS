# 📊 PostgreSQL — Final Summary Table

Based on your uploaded PostgreSQL mock-interview notes, here is the **single final summary table** for quick beginner revision. 

|  # | Topic                   | Definition / Purpose                                         | Main Commands / Functions                                                         | 🧠 Easy Memory             |
| -: | ----------------------- | ------------------------------------------------------------ | --------------------------------------------------------------------------------- | -------------------------- |
|  1 | **Data Types**          | Define what type of data a column can store                  | `INTEGER`, `VARCHAR`, `TEXT`, `NUMERIC`, `DATE`, `TIMESTAMP`, `BOOLEAN`, `SERIAL` | **What type of data?**     |
|  2 | **Constraints**         | Define rules for data in a table                             | `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL`, `CHECK`, `DEFAULT`            | **What rules?**            |
|  3 | **DQL**                 | Retrieve/read data                                           | `SELECT`, `WHERE`, `DISTINCT`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT`          | **Read Data**              |
|  4 | **GROUP BY**            | Creates groups of similar values                             | `GROUP BY column`                                                                 | **Make Groups**            |
|  5 | **ORDER BY**            | Sorts the result                                             | `ASC`, `DESC`                                                                     | **Sort Data**              |
|  6 | **Aggregate Functions** | Calculates results from multiple rows                        | `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()`                                     | **Calculate**              |
|  7 | **WHERE**               | Filters individual rows                                      | `WHERE condition`                                                                 | **Filter Rows**            |
|  8 | **HAVING**              | Filters groups after `GROUP BY`                              | `HAVING condition`                                                                | **Filter Groups**          |
|  9 | **Subquery**            | Query inside another query                                   | `(SELECT ...)`                                                                    | **Query Inside Query**     |
| 10 | **Correlated Subquery** | Inner query depends on outer query                           | Outer-column reference                                                            | **Depends on Outer Query** |
| 11 | **JOIN**                | Combines related data from tables                            | `JOIN ... ON ...`                                                                 | **Combine Tables**         |
| 12 | **INNER JOIN**          | Returns matching rows                                        | `INNER JOIN`                                                                      | **Matching**               |
| 13 | **LEFT JOIN**           | All left rows + matching right rows                          | `LEFT JOIN`                                                                       | **Keep Left**              |
| 14 | **RIGHT JOIN**          | All right rows + matching left rows                          | `RIGHT JOIN`                                                                      | **Keep Right**             |
| 15 | **FULL JOIN**           | All rows from both tables                                    | `FULL OUTER JOIN`                                                                 | **Keep Both**              |
| 16 | **CROSS JOIN**          | Every possible row combination                               | `CROSS JOIN`                                                                      | **Every Combination**      |
| 17 | **SELF JOIN**           | A table joins with itself                                    | `JOIN` + aliases                                                                  | **Same Table**             |
| 18 | **Window Functions**    | Calculates across related rows while keeping individual rows | `FUNCTION() OVER(...)`                                                            | **Calculate + Keep Rows**  |
| 19 | **OVER()**              | Defines the window for calculation                           | `OVER(...)`                                                                       | **Define Window**          |
| 20 | **PARTITION BY**        | Separates rows into groups for window calculation            | `PARTITION BY`                                                                    | **Separate Groups**        |
| 21 | **ROW_NUMBER()**        | Gives each row a unique number                               | `ROW_NUMBER() OVER(...)`                                                          | **Unique Number**          |
| 22 | **RANK()**              | Gives same rank to ties and leaves gaps                      | `RANK() OVER(...)`                                                                | **Rank + Gaps**            |
| 23 | **DENSE_RANK()**        | Gives same rank to ties without gaps                         | `DENSE_RANK() OVER(...)`                                                          | **Rank + No Gaps**         |
| 24 | **SUM() OVER()**        | Calculates total while keeping rows                          | `SUM() OVER(...)`                                                                 | **Window Total**           |
| 25 | **LAG()**               | Gets previous row's value                                    | `LAG()`                                                                           | **Previous**               |
| 26 | **LEAD()**              | Gets next row's value                                        | `LEAD()`                                                                          | **Next**                   |
| 27 | **FIRST_VALUE()**       | Gets first value in the window                               | `FIRST_VALUE()`                                                                   | **First**                  |
| 28 | **LAST_VALUE()**        | Gets last value according to the window frame                | `LAST_VALUE()`                                                                    | **Last**                   |
| 29 | **NTH_VALUE()**         | Gets value from the Nth row                                  | `NTH_VALUE(column,n)`                                                             | **Nth Value**              |
| 30 | **NTILE()**             | Divides rows into buckets                                    | `NTILE(n)`                                                                        | **Buckets**                |
| 31 | **CUME_DIST()**         | Calculates cumulative distribution                           | `CUME_DIST()`                                                                     | **Cumulative**             |
| 32 | **PERCENT_RANK()**      | Calculates relative rank                                     | `PERCENT_RANK()`                                                                  | **Relative Rank**          |
| 33 | **DML**                 | Changes table data                                           | `INSERT`, `UPDATE`, `DELETE`                                                      | **Change Data**            |
| 34 | **DDL**                 | Creates/modifies database structure                          | `CREATE`, `ALTER`, `DROP`, `TRUNCATE`                                             | **Change Structure**       |
| 35 | **DELETE**              | Removes rows                                                 | `DELETE FROM ... WHERE`                                                           | **Remove Rows**            |
| 36 | **TRUNCATE**            | Removes all rows while keeping table                         | `TRUNCATE TABLE`                                                                  | **Empty Table**            |
| 37 | **DROP**                | Removes the table/object itself                              | `DROP TABLE`                                                                      | **Remove Table**           |
| 38 | **Transaction / TCL**   | Treats operations as one unit of work                        | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT`                                        | **Control Changes**        |
| 39 | **BEGIN**               | Starts a transaction                                         | `BEGIN;`                                                                          | **Start**                  |
| 40 | **COMMIT**              | Saves transaction changes                                    | `COMMIT;`                                                                         | **Save**                   |
| 41 | **ROLLBACK**            | Undoes uncommitted changes                                   | `ROLLBACK;`                                                                       | **Undo**                   |
| 42 | **SAVEPOINT**           | Creates a checkpoint in a transaction                        | `SAVEPOINT name`                                                                  | **Checkpoint**             |
| 43 | **DCL**                 | Controls database permissions                                | `GRANT`, `REVOKE`                                                                 | **Permissions**            |
| 44 | **GRANT**               | Gives permissions                                            | `GRANT ...`                                                                       | **Give**                   |
| 45 | **REVOKE**              | Removes permissions                                          | `REVOKE ...`                                                                      | **Remove**                 |
| 46 | **CTE**                 | Named temporary query result                                 | `WITH cte AS (...)`                                                               | **Named Query**            |
| 47 | **Multiple CTE**        | Uses multiple named query steps                              | `WITH cte1 AS (...), cte2 AS (...)`                                               | **Step-by-Step**           |
| 48 | **Procedure**           | Stored program used to perform database operations           | `CREATE PROCEDURE`, `CALL`                                                        | **DO Something**           |
| 49 | **Function**            | Stored routine designed to return a result                   | `CREATE FUNCTION`, `RETURNS`                                                      | **RETURN Something**       |

---

# 🏆 Super Short Revision

| Topic            | Remember              |
| ---------------- | --------------------- |
| **Data Types**   | What type?            |
| **Constraints**  | What rules?           |
| **DQL**          | Read                  |
| **GROUP BY**     | Group                 |
| **ORDER BY**     | Sort                  |
| **Aggregate**    | Calculate             |
| **WHERE**        | Filter rows           |
| **HAVING**       | Filter groups         |
| **Subquery**     | Query inside query    |
| **JOIN**         | Combine tables        |
| **Window**       | Calculate + keep rows |
| **PARTITION BY** | Separate windows      |
| **ROW_NUMBER**   | Unique                |
| **RANK**         | Gaps                  |
| **DENSE_RANK**   | No gaps               |
| **LAG**          | Previous              |
| **LEAD**         | Next                  |
| **FIRST_VALUE**  | First                 |
| **LAST_VALUE**   | Last                  |
| **NTH_VALUE**    | Nth                   |
| **NTILE**        | Buckets               |
| **CTE**          | `WITH` + named query  |
| **DML**          | Change data           |
| **DDL**          | Change structure      |
| **TCL**          | Control transactions  |
| **DCL**          | Control permissions   |
| **Procedure**    | Do                    |
| **Function**     | Return                |

### ⭐ Most important for query-writing practice

**`GROUP BY → HAVING → JOIN → Subquery → Window Functions → CTE`**

These are the areas where you should practice writing queries, not just memorizing definitions. Your notes also identify **Joins, Window Functions, GROUP BY/HAVING, and Subqueries** as especially important for the mock. 
