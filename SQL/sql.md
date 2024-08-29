![logo](mssqlserver-logo.png)

## WIP

- update with a better logo
- create and upload join types images
- indexes code example and maybe some images

## Join types

- **INNER JOIN**: produces a data set that includes rows from the left table, matching rows from the right table.
- **LEFT OUTER JOIN**:  selects data starting from the left table and matching rows in the right table. The left join returns all rows from the left table and the matching rows from the right table. If a row in the left table does not have a matching row in the right table, the columns of the right table will have nulls. The left join is also known as the left outer join. The outer keyword is optional.
- **RIGHT OUTER JOIN**: selects data starting from the right table. It is a reversed version of the left join. The right join returns a result set that contains all rows from the right table and the matching rows in the left table. If a row in the right table does not have a matching row in the left table, all columns in the left table will contain nulls.
- **FULL OUTER JOIN**: returns a result set that contains all rows from both left and right tables, with the matching rows from both sides where available. In case there is no match, the missing side will have NULL values.
- **CROSS JOIN**: also known as a cartesian join, returns all combinations of rows from each table. Envision that you need to find all combinations of size and color. In that case, a CROSS JOIN will be an asset. Note, that this join does not need any condition to join two tables. In fact, CROSS JOIN joins every row from the first table with every row from the second table and its result comprises all combinations of records in two tables. 
- **SELF JOIN**: allows you to join a table to itself. This implies that each row of the table is combined with itself and with every other row of the table.


[Link1](https://www.sqlservertutorial.net/sql-server-basics/sql-server-joins/) - [Link2](https://www.devart.com/dbforge/sql/sqlcomplete/sql-join-statements.html#sql-self-join)

## Indexes

An Index in SQL Server is a data structure associated with tables and views that helps in faster retrieval of rows.

Data in a table is stored in rows in an unordered structure called Heap. If you have to fetch data from a table, the query optimizer has to scan the entire table to retrieve the required row(s). If a table has a large number of rows, then SQL Server will take a long time to retrieve the required rows. So, to speed up data retrieval, SQL Server has a special data structure called indexes.

An index is mostly created on one or more columns which are commonly used in the SELECT clause or WHERE clause.

There are two types of indexes in SQL Server:

### 1.Clustered Indexes: 
Defines the order in which the table data will be sorted and stored. As mentioned before, a table without indexes will be stored in an unordered structure. When you define a clustered index on a column, it will sort data based on that column values and store it. Thus, it helps in faster retrieval of the data.

There can be only one clustered index on a table because the data rows can be stored in only one order.

When you create a Primary Key constraint on a table, a unique clustered index is automatically created on the table.

### 2.Non-Clustered Indexes: 
Does not sort the data rows physically. It creates a separate key-value structure from the table data where the key contains the column values (on which a non-clustered index is declared) and each value contains a pointer to the data row that contains the actual value. It is similar to a textbook having an index at the back of the book with page numbers pointing to the actual information.

There can be 999 non-clustered indexes on a single table.

When you create a Unique constraint, a unique non-clustered index is created on the table.

[Link 1](https://www.tutorialsteacher.com/sqlserver/indexes#:~:text=An%20Index%20in%20SQL%20Server,the%20required%20row(s).) - [Link 2](https://www.tutorialsteacher.com/sqlserver/nonclustered-index)

## Cursor
A database cursor is an object that enables traversal over the rows of a result set. It allows you to process individual row returned by a query.

### Steps for using a cursor
1- Declare
2- Open 
3- Fetch
4- Process
5- Close
6- Deallocate

```SQL
DECLARE cursor_name CURSOR
    FOR select_statement;
    
OPEN cursor_name;

FETCH NEXT FROM cursor INTO variable_list;

WHILE @@FETCH_STATUS = 0  
    BEGIN
        FETCH NEXT FROM cursor_name;  
    END;
    
CLOSE cursor_name;

DEALLOCATE cursor_name;
```
### Example

Sample table **productions.products**

|production.products|
|-------------------|
|* product_id|
|product_name|
|brand_id|
|category_id|
|model_year|
|list_price|

```SQL
DECLARE 
    @product_name VARCHAR(MAX), 
    @list_price   DECIMAL;

DECLARE cursor_product CURSOR
FOR SELECT 
        product_name, 
        list_price
    FROM 
        production.products;

OPEN cursor_product;

FETCH NEXT FROM cursor_product INTO 
    @product_name, 
    @list_price;

WHILE @@FETCH_STATUS = 0
    BEGIN
        PRINT @product_name + CAST(@list_price AS varchar);
        FETCH NEXT FROM cursor_product INTO 
            @product_name, 
            @list_price;
    END;

CLOSE cursor_product;

DEALLOCATE cursor_product;
```

[Link1](https://www.sqlservertutorial.net/sql-server-stored-procedures/sql-server-cursor/)
## Delete vs truncate
| SQL Delete                                                                                                                             | SQL Truncate                                                                                                                                                                                        |
|----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Delete command is useful to delete all or specific rows from a table specified using a Where clause                                    | The truncate command removes all rows of a table. We cannot use a Where clause in this.                                                                                                             |
| It is a DML command                                                                                                                    | It is a DDL command.                                                                                                                                                                                |
| SQL Delete command places lock on each row requires to delete from a table.                                                            | SQL Truncate command places a table and page lock to remove all records.                                                                                                                            |
| Delete command logs entry for each deleted row in the transaction log.                                                                 | The truncate command does not log entries for each deleted row in the transaction log.                                                                                                              |
| Delete command is slower than the Truncate command.                                                                                    | It is faster than the delete command.                                                                                                                                                               |
| It removes rows one at a time.                                                                                                         | It removes all rows in a table by deallocating the pages that are used to store the table data                                                                                                      |
| It retains the identity and does not reset it to the seed value.                                                                       | Truncate command reset the identity to its seed value.                                                                                                                                              |
| It requires more transaction log space than the truncate command.                                                                      | It requires less transaction log space than the truncate command.                                                                                                                                   |
| You require delete permission on a table to use this                                                                                   | You require Alter table permissions to truncate a table.                                                                                                                                            |
| You can use the Delete statement with the indexed views.                                                                               | You cannot use the truncate command with the indexed views.                                                                                                                                         |
| Delete command retains the object statistics and allocated space.                                                                      | Truncate deallocates all data pages of a table. Therefore, it removes all statistics and allocated space as well.                                                                                   |
| Delete command can activate a trigger as well. Delete works on individual rows and delete the data. Therefore, it activates a trigger. | The truncate command cannot activate a trigger. The trigger is activated if any row modification takes place. In this command, SQL Server deallocates all pages, so it does not activate a trigger. |
| Delete command removes the rows matched with the where clause. It also does not remove the columns, indexes, constraints, schema       | The truncate command only removes all rows of a table. It does not remove the columns, indexes, constraints, and schema.                                                                            |

[Link 1](https://www.sqlshack.com/difference-between-sql-truncate-and-sql-delete-statements-in-sql-server/) - [Link 2](https://www.tutorialspoint.com/difference-between-delete-and-truncate-in-sql-query#:~:text=The%20DELETE%20command%20in%20SQL,not%20any%20conditions%20are%20met.)

## Key Differences Between Stored Procedures and Functions

### When to Use Stored Procedures vs. Functions
| **Aspect**                         | **Stored Procedure**                                                                                 | **Function**                                                                                         |
|------------------------------------|------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| **Purpose**                        | Used to perform one or more tasks such as DML (Data Manipulation Language) operations, logic execution, etc. | Used to compute and return a single value or result set. Primarily used for calculations and operations. |
| **Return Value**                   | May or may not return a value. Can return multiple values using `OUT` parameters or result sets.       | Must return a single value (scalar, table, or complex data type).                                     |
| **Invocation**                     | Called using the `EXEC` or `CALL` statement.                                                         | Called as part of an expression (e.g., in a `SELECT` statement) or using a function call.             |
| **Output Parameters**              | Can have output (`OUT`) parameters to return multiple values.                                          | Cannot have output parameters; returns only one value through the `RETURN` statement.                 |
| **Use in SQL Statements**          | Cannot be directly used in SQL statements like `SELECT`, `WHERE`, or `JOIN`.                           | Can be used directly in SQL statements such as `SELECT`, `WHERE`, `JOIN`, etc.                        |
| **Transaction Handling**           | Can contain transaction management code (e.g., `BEGIN TRANSACTION`, `COMMIT`, `ROLLBACK`).            | Cannot contain explicit transaction management code; it's meant for computation and must be deterministic. |
| **Side Effects**                   | Can perform operations that cause side effects (e.g., inserting, updating, deleting data).            | Should not cause side effects. Generally used for read-only operations or calculations.               |
| **Exception Handling**             | Supports exception handling using `TRY...CATCH` blocks in SQL Server or similar mechanisms in other RDBMS. | Typically does not support exception handling mechanisms like stored procedures.                      |
| **Return Data Type**               | Does not have a return data type defined; can return result sets via `SELECT` or output parameters.    | Must have a defined return data type, such as `int`, `varchar`, `table`, etc.                         |
| **Performance Optimization**       | Often optimized for larger and more complex tasks that might include multiple operations.             | Optimized for computations that are performed frequently and are deterministic.                       |
| **Compatibility with Triggers**    | Cannot be directly called in a trigger.                                                               | Can be called from a trigger.                                                                         |


#### Use Stored Procedures When:
- You need to perform complex operations, including multiple DML operations (insert, update, delete).
- You need to perform a series of operations or business logic that may include conditional logic, loops, and exception handling.
- You need to use transactions or manage error handling within the SQL code.
- You need to return multiple result sets or affect the database state directly.

 #### Use Functions When:
- You need to perform a calculation or return a single value that can be used in SQL statements.
- You want to encapsulate reusable code that does not affect the state of the database.
- You need to use a value or table as part of a SELECT, WHERE, or JOIN clause.
- You want to ensure the code is deterministic (i.e., given the same inputs, the same output will always be produced).

Here's the content with tables in Markdown format, prepared for you to copy and paste directly into a `.md` file:

---

## **Varchar vs Nvarchar**
| **Aspect**         | **VARCHAR**                                                       | **NVARCHAR**                                                      |
|--------------------|-------------------------------------------------------------------|-------------------------------------------------------------------|
| **Definition**     | Variable-length, non-Unicode character data.                       | Variable-length, Unicode character data.                          |
| **Storage Size**   | Uses 1 byte per character.                                         | Uses 2 bytes per character.                                       |
| **Use Case**       | For non-Unicode data (e.g., English text).                         | For Unicode data (e.g., text in multiple languages).              |
| **Performance**    | Faster and uses less storage for non-Unicode data.                 | Slightly slower and uses more storage due to Unicode support.     |
| **Capacity**       | Maximum 8,000 characters (SQL Server).                             | Maximum 4,000 characters (SQL Server).                            |
| **Collation**      | Uses non-Unicode collation settings.                               | Uses Unicode collation settings.                                  |

---

## **Cross Apply**

`CROSS APPLY` is used to join a table-valued function with an outer table in SQL Server. It returns only those rows from the outer table for which the table-valued function returns a result.

| **Use Case**                          | **Description**                                                                 |
|---------------------------------------|---------------------------------------------------------------------------------|
| **Table-Valued Functions**            | Joins rows from a table to rows returned by a table-valued function.             |
| **Performance**                       | Often used for better performance when a table-valued function is required.      |
| **Comparison with `OUTER APPLY`**     | `CROSS APPLY` returns rows only when a match is found, unlike `OUTER APPLY`.     |

---

## **Scan vs Seek**

| **Aspect**          | **Index Scan**                                                             | **Index Seek**                                                            |
|---------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------|
| **Definition**      | Reads all rows in an index or table to find the relevant data.             | Uses index b-tree structure to directly find specific rows.              |
| **Use Case**        | When a query returns a large portion of the table.                         | When a query returns a small number of rows using indexed columns.       |
| **Performance**     | Slower, more I/O operations as it scans the entire table or index.         | Faster, fewer I/O operations as it directly accesses the data.           |
| **Cost**            | Higher cost due to scanning all rows.                                      | Lower cost due to direct access to the needed rows.                      |

---

## **Where vs Having**

| **Aspect**          | **WHERE Clause**                                                           | **HAVING Clause**                                                        |
|---------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------|
| **Purpose**         | Filters rows before grouping them.                                         | Filters groups after the aggregation has been performed.                 |
| **Usage**           | Used with `SELECT`, `UPDATE`, `DELETE` statements.                         | Used only with `SELECT` statements and must be used with `GROUP BY`.     |
| **Performance**     | Faster as it filters records before grouping.                              | Slower as it filters after aggregation.                                  |
| **Syntax**          | `SELECT * FROM table WHERE condition;`                                      | `SELECT * FROM table GROUP BY column HAVING condition;`                   |

---

## **Common Table Expressions (CTEs)**

Common Table Expressions (CTEs) are a temporary result set that can be referenced within a `SELECT`, `INSERT`, `UPDATE`, or `DELETE` statement.

| **Aspect**          | **Description**                                                             |
|---------------------|-----------------------------------------------------------------------------|
| **Definition**      | A temporary named result set derived from a simple query.                    |
| **Syntax**          | `WITH CTE_Name AS (SELECT * FROM table WHERE condition)`                     |
| **Use Case**        | Used for complex queries to improve readability and maintainability.         |
| **Recursion**       | Supports recursive queries (e.g., hierarchical data retrieval).              |

---

## **Find Second, Nth Highest Salary**

To find the second or Nth highest salary in SQL Server:

```sql
SELECT DISTINCT Salary 
FROM Employees E1 
WHERE N-1 = (SELECT COUNT(DISTINCT Salary) 
             FROM Employees E2 
             WHERE E2.Salary > E1.Salary);
```

---

## **Difference between `RANK()`, `DENSE_RANK()`, and `ROW_NUMBER()`**

| **Function**      | **Description**                                                                 |
|-------------------|---------------------------------------------------------------------------------|
| **`RANK()`**      | Assigns a rank to each row within a partition, with gaps for duplicate rankings. |
| **`DENSE_RANK()`**| Assigns a rank without gaps; duplicate rankings have the same number, no gaps.   |
| **`ROW_NUMBER()`**| Assigns a unique sequential integer to rows within a partition without gaps.     |

---

## **Update Table to Restructure Rows to Columns and Values**

Using `PIVOT` to convert rows to columns:

```sql
SELECT *
FROM (SELECT ColumnName, Value FROM TableName) AS SourceTable
PIVOT (MAX(Value) FOR ColumnName IN ([Column1], [Column2], [Column3])) AS PivotTable;
```

---

## **Find and Delete Duplicates from a Table**

To delete duplicate rows:

```sql
WITH CTE AS (
  SELECT *, 
         ROW_NUMBER() OVER (PARTITION BY Column1, Column2 ORDER BY (SELECT NULL)) AS RowNum
  FROM TableName
)
DELETE FROM CTE WHERE RowNum > 1;
```

---

## **Find Unique or Identical Records Within 2 Tables**

```sql
SELECT * FROM Table1
EXCEPT
SELECT * FROM Table2;

SELECT * FROM Table1
INTERSECT
SELECT * FROM Table2;
```

---

## **Number of Records in Output for Various Joins**

| **Join Type**    | **Description**                                                     | **Number of Records**                                                         |
|------------------|---------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **INNER JOIN**   | Returns records with matching values in both tables.                | Matches only.                                                                 |
| **LEFT JOIN**    | Returns all records from the left table, and matched records from the right table. | All from the left table, matched from right.                                   |
| **RIGHT JOIN**   | Returns all records from the right table, and matched records from the left table. | All from the right table, matched from left.                                   |
| **FULL JOIN**    | Returns all records when there is a match in either left or right table. | All records from both tables.                                                 |
| **CROSS JOIN**   | Returns the Cartesian product of both tables.                       | Number of rows in the first table * number of rows in the second table.        |

---

## **Find Employees' Salary and Compare Higher Ones with Manager's Salary**

```sql
SELECT E.EmployeeID, E.Salary, M.ManagerID, M.Salary AS ManagerSalary
FROM Employees E
JOIN Employees M ON E.ManagerID = M.EmployeeID
WHERE E.Salary > M.Salary;
```

---

## **Department-Wise Highest Salary**

```sql
SELECT Department, MAX(Salary) AS HighestSalary
FROM Employees
GROUP BY Department;
```

---

## **Difference between `UNION` and `UNION ALL`**

| **Aspect**     | **UNION**                                        | **UNION ALL**                                   |
|----------------|--------------------------------------------------|-------------------------------------------------|
| **Duplicates** | Removes duplicates from the result set.          | Includes all duplicates in the result set.       |
| **Performance**| Slower due to duplicate removal.                 | Faster as it doesn't remove duplicates.          |
| **Use Case**   | When unique records are needed.                  | When duplicates are acceptable or expected.      |

---

## **Difference between `GROUP BY` and `ORDER BY`**

| **Aspect**     | **GROUP BY**                                         | **ORDER BY**                                        |
|----------------|------------------------------------------------------|-----------------------------------------------------|
| **Purpose**    | Groups rows that have the same values in specified columns. | Sorts the result set by one or more columns.         |
| **Use Case**   | Used with aggregate functions like `SUM`, `COUNT`.   | Used to sort results in ascending or descending order. |
| **Syntax**     | `SELECT column, SUM(value) FROM table GROUP BY column` | `SELECT * FROM table ORDER BY column ASC/DESC`       |

---

## **Difference between Partition, Clustering, Bucketing Approaches**

| **Aspect**        | **Partitioning**                                                      | **Clustering**                                                | **Bucketing**                                          |
|-------------------|----------------------------------------------------------------------|--------------------------------------------------------------|-------------------------------------------------------|
| **Definition**    | Dividing a table into smaller, more manageable pieces.                | Sorting data in a particular order for efficient retrieval.   | Dividing data into fixed-size buckets for better query performance. |
| **Use Case**      | Improves query performance by reducing the amount of data scanned.    | Optimizes range queries and enhances query performance.       | Speeds up joins and aggregations on specific columns.  |
| **Example**       | Monthly partitions for sales data.                                    | Clustering by customer ID for faster retrieval.               | Bucketing on employee age for faster aggregation.      |

---

## **What is an Index and How Does it Improve Performance**

| **Aspect**            

 | **Description**                                                              |
|------------------------|------------------------------------------------------------------------------|
| **Definition**         | A database object that improves the speed of data retrieval operations.       |
| **Types**              | Clustered, Non-Clustered, Full-Text, XML, Spatial.                            |
| **Performance**        | Reduces I/O operations and speeds up query performance by using a b-tree structure. |
| **Trade-Off**          | Increases write operation time and requires more storage space.               |

---

## **Performance Optimization Approaches in SQL**

| **Approach**           | **Description**                                                                  |
|------------------------|----------------------------------------------------------------------------------|
| **Indexing**           | Use indexes to speed up read operations.                                          |
| **Query Optimization** | Write efficient SQL queries, avoid `SELECT *`, and use appropriate joins.         |
| **Partitioning**       | Divide large tables into smaller, manageable pieces.                              |
| **Normalization**      | Structure the database to reduce redundancy and improve data integrity.           |
| **Caching**            | Store frequently accessed data in memory to reduce database load.                 |

---

## **What is a Subquery and How is it Different from a Join**

| **Aspect**         | **Subquery**                                                               | **Join**                                                                        |
|--------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| **Definition**     | A query nested inside another query.                                       | Combines columns from two or more tables based on a related column between them. |
| **Use Case**       | Used for filtering, aggregation, and calculations.                         | Used for combining data from multiple tables.                                    |
| **Performance**    | Can be slower due to nested execution.                                     | Usually faster as it combines rows in a single pass.                             |
| **Syntax**         | `SELECT * FROM table1 WHERE column1 = (SELECT column2 FROM table2)`         | `SELECT * FROM table1 JOIN table2 ON table1.id = table2.id`                      |

---

## **Difference between a Clustered and Non-Clustered Index**

| **Aspect**              | **Clustered Index**                                                   | **Non-Clustered Index**                                                          |
|-------------------------|----------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **Definition**          | Reorders the physical order of the table and is the table itself.     | Creates a separate object within the table that points back to the table data.    |
| **Number Allowed**      | Only one clustered index per table.                                   | Multiple non-clustered indexes are allowed per table.                             |
| **Performance**         | Faster for range queries.                                             | Slower for retrieving data directly; better for exact match queries.              |

---

## **Types of Constraints in SQL**

| **Constraint**      | **Description**                                                                 |
|---------------------|---------------------------------------------------------------------------------|
| **PRIMARY KEY**     | Uniquely identifies each record in a table.                                      |
| **FOREIGN KEY**     | Ensures referential integrity between two tables.                                |
| **UNIQUE**          | Ensures all values in a column are unique.                                       |
| **CHECK**           | Ensures all values in a column satisfy a specific condition.                     |
| **DEFAULT**         | Sets a default value for a column when no value is specified.                    |
| **NOT NULL**        | Ensures a column cannot have a NULL value.                                       |

---

## **Difference between DELETE and TRUNCATE Commands in SQL**

| **Aspect**          | **DELETE**                                                        | **TRUNCATE**                                                     |
|---------------------|------------------------------------------------------------------|------------------------------------------------------------------|
| **Usage**           | Removes rows based on a condition or all rows without a condition. | Removes all rows from a table without logging individual row deletions. |
| **Logging**         | Fully logged; can be rolled back.                                 | Minimal logging; cannot be rolled back.                           |
| **Performance**     | Slower due to logging and triggers.                               | Faster as it resets the table without individual row deletions.   |
| **Trigger Activation** | Activates triggers.                                           | Does not activate triggers.                                       |

---

## **Difference between Trigger and Stored Procedure**

| **Aspect**              | **Trigger**                                                               | **Stored Procedure**                                               |
|-------------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------|
| **Definition**          | A set of SQL statements that automatically execute in response to certain events. | A set of SQL statements that execute when explicitly called.       |
| **Use Case**            | Automatically enforce business rules, auditing, or logging.               | Perform routine tasks, complex logic, and data manipulation.       |
| **Execution**           | Automatically invoked by DML events (INSERT, UPDATE, DELETE).             | Manually invoked using `EXEC` or `CALL`.                           |

---

## **Difference between a NULL Value and a Zero Value in SQL**

| **Aspect**              | **NULL Value**                                                | **Zero Value**                                                   |
|-------------------------|--------------------------------------------------------------|------------------------------------------------------------------|
| **Definition**          | Represents an unknown, undefined, or missing value.           | Represents a numerical value of zero.                            |
| **Data Type Compatibility** | Can be used with any data type.                           | Only compatible with numeric data types.                         |
| **Comparison Behavior** | Any comparison with `NULL` results in `UNKNOWN`.              | Can be compared directly with other numeric values.              |
| **Storage**             | Requires special handling and storage.                        | Stored as a regular numeric value.                               |

Feel free to copy and paste this content into your Markdown file for documentation or study purposes.
