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
