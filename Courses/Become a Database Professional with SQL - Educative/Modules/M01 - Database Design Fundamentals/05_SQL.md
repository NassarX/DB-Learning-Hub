# ****Structured Query Language (SQL)****

**What is SQL?**

SQL is Structured Query Language, which is a computer language for storing, manipulating and retrieving data in a relational database.

****SQL Commands****

- **DDL - Data Definition Language**
    
    
| Command | Description |
| --- | --- |
| CREATE | Creates a new table, a view of a table, or other objects in the database. |
| ALTER | Modifies an existing database object, such as a table. |
| DROP | Deletes an entire table, a view of a table or other objects in the database. |
- **DML - Data Manipulation Language**
    
    
| Command | Description |
| --- | --- |
| SELECT | Retrieves certain records from one or more tables. |
| INSERT | Creates a record. |
| UPDATE | Modifies records. |
| DELETE | Deletes records. |

****SQL data types****

- **Exact Numeric Data Types**
    
    
| Data Type | Ranges From | To |
| --- | --- | --- |
| int | -2,147,483,648 | 2,147,483,647 |
| bigint | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 |
| smallint | -32,768 | 32,767 |
| tinyint | 0 | 255 |
| bit | 0 | 1 |
| decimal | -10^38 +1 | 10^38 -1 |
| numeric | -10^38 +1 | 10^38 -1 |
- **Approximate Numeric Data Types**
    
    
| Data Type | Ranges From | To |
| --- | --- | --- |
| float | -1.79E + 30 | 1.79E + 308 |
| real | -3.40E + 38 | 3.40E + 38 |
- **Date and Time Data Types**
    
    
| Data Type | Ranges From | To |
| --- | --- | --- |
| datetime | Jan 1, 1753 | Dec 31, 9999 |
| smalldatetime | Jan 1, 1900 | Jun 6, 2079 |
| date | Stores a date like June 30, 1991 | - |
| time | Stores a time like 12:30 P.M. |  |
- **Character Strings Data Types**
    
    
    | DATA TYPE | Description |
    | --- | --- |
    | char | Maximum length of 8,000 characters.( Fixed length non-Unicode characters) |
    | varchar | Maximum of 8,000 characters. (Variable-length non-Unicode data). |
    | varchar(max) | Maximum length of 2E + 31 characters, Variable-length non-Unicode data. |
    | text | Variable-length non-Unicode data with a maximum length of 2,147,483,647 characters. |

****SQL constraint****

- NOT NULL
- DEFAULT
- UNIQUE
- PRIMARY Key
- FORIGN Key
- CHECK

**SQL Aggregate functions**

- `COUNT()` → Returns the number of `Non-Null`
 values in the specified column.
    
    ```sql
    SELECT COUNT(column_name) FROM table_name WHERE condition;
    ```
    
- `SUM()` → Returns the total sum of a numeric column.
    
    ```sql
    SELECT SUM(column_name) FROM table_name WHERE condition;
    ```
    
- `AVG()` → Returns the average value of a numeric column.
    
    ```sql
    SELECT AVG(column_name) FROM table_name WHERE condition;
    ```
    
- `MIN()` → Returns the smallest value in the selected column.
    
    ```sql
    SELECT MIN(column_name) FROM table_name WHERE condition;
    ```
    
- `MAX()` → Returns the largest value of the selected column.
    
    ```sql
    SELECT MAX(column_name) FROM table_name WHERE condition;
    ```
    

**The ORDER BY clause**

```sql
SELECT column-list FROM table_name WHERE condition ORDER BY column1, column2, .. columnN;
```

- Some databases sort the query results in ascending order by default.
- Make sure whatever column you are using to sort is in the column-list.

****The GROUP BY clause****

```sql
SELECT column-list FROM table_name WHERE conditions
GROUP BY column1, column2 ... columnN
ORDER BY column1, column2 ... columnN;
```

- The `GROUP BY` clause must follow the conditions in the `WHERE` clause and must precede the `ORDER BY` clause if one is used.

****The HAVING clause****

```sql
SELECT column-list FROM table_name WHERE [ conditions ]
GROUP BY column1, column2 HAVING [ conditions ]
ORDER BY column1, column2;
```

- Only returns rows where aggregate function results are matched with given conditions.
- As you can see, the `HAVING` clause must follow the `GROUP BY`clause in a query and must also precede the `ORDER BY` clause if used.

```sql
SELECT ADDRESS, COUNT(ID) FROM CUSTOMERS
GROUP BY ADDRESS HAVING COUNT(ID) > 2;
```

****Alias syntax****

```sql
SELECT column1, column2 FROM table_name AS alias_name WHERE condition;
```

```sql
SELECT column_name AS alias_name FROM table_name WHERE condition;
```

****SQL JOIN****

- **INNER JOIN / JOIN**: Returns records that have matching values in both tables.
    
    ```sql
    SELECT table1.column1, table2.column2 FROM table1 INNER JOIN table2
    ON table1.common_field = table2.common_field;
    ```
    
- **LEFT JOIN/ LEFT OUTER JOIN**: Returns all records from the left table, and the matched records from the right table.
    
    ```sql
    SELECT table1.column1, table2.column2 FROM table1
    LEFT JOIN table2 ON table1.common_field = table2.common_field;
    ```
    
- **RIGHT JOIN/ RIGHT OUTER**: Returns all records from the right table, and the matched records from the left table.
    
    ```sql
    SELECT table1.column1, table2.column2 FROM table1
    RIGHT JOIN table2 ON table1.common_field = table2.common_field;
    ```