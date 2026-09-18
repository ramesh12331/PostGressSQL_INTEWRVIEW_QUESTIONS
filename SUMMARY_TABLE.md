# 📊 PostgreSQL — Final Summary Table

|  # | Topic               | Definition                                   | Main Purpose                             | Important Syntax / Functions        | 🧠 Easy Memory            |
| -: | ------------------- | -------------------------------------------- | ---------------------------------------- | ----------------------------------- | ------------------------- |
|  1 | **GROUP BY**        | Groups rows having the same values           | Calculate results group-wise             | `GROUP BY column`                   | **Make Groups**           |
|  2 | **ORDER BY**        | Sorts query results                          | Arrange data ascending/descending        | `ORDER BY column DESC/ASC`          | **Sort**                  |
|  3 | **WHERE**           | Filters individual rows                      | Get only required rows                   | `WHERE condition`                   | **Filter Rows**           |
|  4 | **HAVING**          | Filters groups after grouping                | Filter aggregate results                 | `HAVING SUM(...) > ...`             | **Filter Groups**         |
|  5 | **INNER JOIN**      | Returns matching rows from tables            | Combine related data                     | `JOIN ... ON ...`                   | **Matching**              |
|  6 | **LEFT JOIN**       | All left rows + matching right rows          | Keep all records from left table         | `LEFT JOIN ... ON ...`              | **Keep Left**             |
|  7 | **RIGHT JOIN**      | All right rows + matching left rows          | Keep all records from right table        | `RIGHT JOIN ... ON ...`             | **Keep Right**            |
|  8 | **FULL JOIN**       | All rows from both tables                    | Keep matching + non-matching rows        | `FULL OUTER JOIN`                   | **Keep Both**             |
|  9 | **CROSS JOIN**      | Every row combined with every row            | Create all combinations                  | `CROSS JOIN`                        | **Every Combination**     |
| 10 | **SELF JOIN**       | Table joined with itself                     | Employee-manager type relationships      | `JOIN same_table`                   | **Same Table**            |
| 11 | **Window Function** | Calculates across rows without reducing them | Ranking, totals, previous/next values    | `FUNCTION() OVER(...)`              | **Calculate + Keep Rows** |
| 12 | **PARTITION BY**    | Divides rows into windows/groups             | Perform calculation separately per group | `OVER(PARTITION BY dept)`           | **Separate Groups**       |
| 13 | **ROW_NUMBER()**    | Gives unique sequential numbers              | Number rows                              | `ROW_NUMBER() OVER(...)`            | **Unique Number**         |
| 14 | **RANK()**          | Gives ranking with gaps for ties             | Rank records                             | `RANK() OVER(...)`                  | **Rank + Gaps**           |
| 15 | **DENSE_RANK()**    | Gives ranking without gaps                   | Rank records                             | `DENSE_RANK() OVER(...)`            | **Rank + No Gaps**        |
| 16 | **LAG()**           | Gets previous row's value                    | Compare with previous record             | `LAG(column) OVER(...)`             | **Previous**              |
| 17 | **LEAD()**          | Gets next row's value                        | Compare with next record                 | `LEAD(column) OVER(...)`            | **Next**                  |
| 18 | **FIRST_VALUE()**   | Gets first value in window                   | Find first/highest according to ordering | `FIRST_VALUE(column) OVER(...)`     | **First**                 |
| 19 | **LAST_VALUE()**    | Gets last value in window frame              | Find last value                          | `LAST_VALUE(column) OVER(...)`      | **Last**                  |
| 20 | **NTH_VALUE()**     | Gets value from Nth row                      | Find 2nd/3rd/etc. value                  | `NTH_VALUE(column, n)`              | **Nth**                   |
| 21 | **NTILE()**         | Divides rows into buckets                    | Create price/score groups                | `NTILE(3) OVER(...)`                | **Buckets**               |
| 22 | **CUME_DIST()**     | Calculates cumulative distribution           | Distribution/percentage analysis         | `CUME_DIST() OVER(...)`             | **Cumulative**            |
| 23 | **PERCENT_RANK()**  | Calculates relative rank                     | Percentage-based ranking                 | `PERCENT_RANK() OVER(...)`          | **Relative Rank**         |
| 24 | **Subquery**        | Query inside another query                   | Use one query's result in another        | `WHERE x = (SELECT ...)`            | **Query Inside Query**    |
| 25 | **CTE**             | Temporary named query result                 | Organize complex queries                 | `WITH cte AS (...)`                 | **Named Query Result**    |
| 26 | **Multiple CTE**    | Multiple CTEs in one query                   | Break complex logic into steps           | `WITH cte1 AS (...), cte2 AS (...)` | **Step-by-Step Query**    |
| 27 | **Procedure**       | Stored database program                      | Perform database operations              | `CREATE PROCEDURE`                  | **DO Something**          |
| 28 | **CALL**            | Executes a procedure                         | Run stored procedure                     | `CALL procedure_name()`             | **Execute**               |
| 29 | **Function**        | Stored routine designed to return a result   | Calculate/return data                    | `CREATE FUNCTION`                   | **RETURN Something**      |
| 30 | **RETURNS TABLE**   | Defines table-shaped function output         | Return multiple rows/columns             | `RETURNS TABLE (...)`               | **Return Table**          |

---

# ⭐ Most Important Differences

| Concept 1    | Concept 2       | Main Difference                                            |
| ------------ | --------------- | ---------------------------------------------------------- |
| `GROUP BY`   | Window Function | GROUP BY reduces rows; Window Function keeps rows          |
| `WHERE`      | `HAVING`        | WHERE filters rows; HAVING filters groups                  |
| `INNER JOIN` | `LEFT JOIN`     | INNER keeps matches; LEFT keeps all left rows              |
| `RANK()`     | `DENSE_RANK()`  | RANK has gaps; DENSE_RANK has no gaps                      |
| `LAG()`      | `LEAD()`        | LAG = previous; LEAD = next                                |
| Subquery     | CTE             | Subquery = nested query; CTE = named query using `WITH`    |
| Procedure    | Function        | Procedure performs an operation; Function returns a result |

---

# 🧠 One-Line Final Memory

| Topic               | Remember This                   |
| ------------------- | ------------------------------- |
| **GROUP BY**        | 🟦 Make Groups                  |
| **ORDER BY**        | 🔽 Sort                         |
| **WHERE**           | 🔍 Filter Rows                  |
| **HAVING**          | 🔍 Filter Groups                |
| **JOIN**            | 🔗 Combine Tables               |
| **PARTITION BY**    | 📦 Separate Groups              |
| **Window Function** | 🪟 Calculate + Keep Rows        |
| **ROW_NUMBER**      | 🔢 Unique Number                |
| **RANK**            | 🏆 Rank + Gaps                  |
| **DENSE_RANK**      | 🏆 Rank + No Gaps               |
| **LAG**             | ⬅️ Previous                     |
| **LEAD**            | ➡️ Next                         |
| **FIRST_VALUE**     | 1️⃣ First                       |
| **LAST_VALUE**      | 🔚 Last                         |
| **NTH_VALUE**       | 🔢 Nth                          |
| **NTILE**           | 🪣 Buckets                      |
| **CUME_DIST**       | 📈 Cumulative                   |
| **PERCENT_RANK**    | 📊 Relative Rank                |
| **Subquery**        | 🔄 Query Inside Query           |
| **CTE**             | 📝 Named Temporary Query Result |
| **Procedure**       | ⚙️ Do Something                 |
| **CALL**            | ▶️ Execute Procedure            |
| **Function**        | ↩️ Return Something             |

---

# 🔥 SQL Query Flow

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
ORDER BY
 ↓
LIMIT
```

### 🏆 Golden Formula

```text
WHERE      → ROWS
GROUP BY   → GROUPS
HAVING     → GROUPS FILTER
ORDER BY   → SORT
JOIN       → COMBINE
WINDOW     → CALCULATE + KEEP
SUBQUERY   → QUERY INSIDE QUERY
CTE        → WITH + NAMED QUERY
PROCEDURE  → DO + CALL
FUNCTION   → RETURN
```

**This is the final beginner-level revision table for your PostgreSQL mock interview.**
