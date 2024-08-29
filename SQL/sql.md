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

## Varchar vs nvarchar

## Cross Apply

## Scan vs seek

## Where vs having

## CTEs

## Find second,nth highest salary

## Difference between rank,dense_rank and row_number

## Update table to restructure rows to columns and values

## Find and delete duplicates from a table

## find unique or identical records within 2 tables

## Number of records in output for various joins

## Find employees salary and compare higher one’s with manager’s salary

## Department wise highest salary

## Difference between union and union all 

## Difference between GROUP BY and ORDER BY

##  Difference between Partition, clustering, bucketing approaches

## What is an index and how does it improve performance

## Performance optimization approaches in SQL

## What is a subquery and how is it different from a join

## Difference between a clustered and non-clustered index

## What are the different types of constraints in SQL, and how are they used?

## Difference between DELETE and TRUNCATE commands in SQL

## Difference between Trigger and Stored Procedure

## Difference between a NULL value and a zero value in SQL
