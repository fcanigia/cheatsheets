![logo](mssqlserver-logo.png)

## WIP

- update with a better logo
- create and upload join types images

## Join types

- **INNER JOIN**: produces a data set that includes rows from the left table, matching rows from the right table.
- **LEFT OUTER JOIN**:  selects data starting from the left table and matching rows in the right table. The left join returns all rows from the left table and the matching rows from the right table. If a row in the left table does not have a matching row in the right table, the columns of the right table will have nulls. The left join is also known as the left outer join. The outer keyword is optional.
- **RIGHT OUTER JOIN**: selects data starting from the right table. It is a reversed version of the left join. The right join returns a result set that contains all rows from the right table and the matching rows in the left table. If a row in the right table does not have a matching row in the left table, all columns in the left table will contain nulls.
- **FULL OUTER JOIN**: returns a result set that contains all rows from both left and right tables, with the matching rows from both sides where available. In case there is no match, the missing side will have NULL values.
- **CROSS JOIN**: also known as a cartesian join, returns all combinations of rows from each table. Envision that you need to find all combinations of size and color. In that case, a CROSS JOIN will be an asset. Note, that this join does not need any condition to join two tables. In fact, CROSS JOIN joins every row from the first table with every row from the second table and its result comprises all combinations of records in two tables. 
- **SELF JOIN**: allows you to join a table to itself. This implies that each row of the table is combined with itself and with every other row of the table.


[Link1](https://www.sqlservertutorial.net/sql-server-basics/sql-server-joins/) - [Link2](https://www.devart.com/dbforge/sql/sqlcomplete/sql-join-statements.html#sql-self-join)

## Indexes

## Cursor

## Delete vs truncate

## Varchar vs nvarchar

## Cross Apply

## Scan vs seek

## Where vs having

## CTEs
