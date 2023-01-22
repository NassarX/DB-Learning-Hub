# ****Exploring MySQL****

```sql
-- Query 1
SHOW DATABASES;

-- Query 2
USE mysql;

-- Query 3
SHOW CREATE DATABASE mysql;

-- Query 4
SHOW TABLES;

-- Query 5
DESCRIBE user;

-- Query 6
SHOW CREATE TABLE servers;

-- Query 7
SHOW COLUMNS FROM servers;
```

- **Database Commands**

```sql
-- Query 1
CREATE DATABASE MovieIndustry;

-- Query 2
CREATE DATABASE IF NOT EXISTS MovieIndustry;

-- Query 3
SHOW DATABASES;

-- Query 4
DROP DATABASE MovieIndustry;
```

- **Tables Creation**

```sql
-- Query 1
CREATE TABLE Actors (
Id INT AUTO_INCREMENT,
FirstName VARCHAR(20) NOT NULL,
SecondName VARCHAR(20) NOT NULL,
DoB DATE NOT NULL,
Gender ENUM('Male','Female','Other') NOT NULL,
MaritalStatus ENUM('Married', 'Divorced', 'Single', 'Unknown') DEFAULT "Unknown",
NetWorthInMillions DECIMAL NOT NULL,
PRIMARY KEY (Id));

-- Query 2
CREATE TABLE IF NOT EXISTS Actors (
Id INT AUTO_INCREMENT,
FirstName VARCHAR(20) NOT NULL,
SecondName VARCHAR(20) NOT NULL,
DoB DATE NOT NULL,
Gender ENUM('Male','Female','Other') NOT NULL,
MaritalStatus ENUM('Married', 'Divorced', 'Single', 'Unknown') DEFAULT "Unknown",
NetWorthInMillions DECIMAL NOT NULL,
PRIMARY KEY (Id));
```

- **AUTO_INCREMENT** → Automatically assigns the next integer in the sequence to the ID column of a new row.
    - There can be only one column marked as **AUTO_INCREMENT** in a table.
    - The **AUTO_INCREMENT** column can’t have a default value.
    - The **AUTO_INCREMENT** column must be indexed.
    - The **AUTO_INCREMENT** feature isn’t portable to other databases and the counter is reset when we truncate or drop a table.


**Instead of listing all known and easy to remember SQL gems, I'm gonna some of hidden gems I've found during the course :**

- Various operators that can be used in a WHERE clause.

| Operator            | Purpose                                                                  |
|---------------------|--------------------------------------------------------------------------|
| <=>                 | NULL-safe equal to operator                                              |
| BETWEEN … AND …     | Whether a value is within a range of values                              |
| COALESCE()          | Return the first non-NULL argument                                       |
| GREATEST()          | Return the largest argument                                              |
| INTERVAL            | Return the index of the argument that is greater than the first argument |
| IS                  | Test a value against a boolean                                           |
| IS NOT              | Test a value against a boolean                                           |
| ISNULL()            | Test whether the argument is NULL                                        |
| LEAST()             | Return the smallest argument                                             |
| NOT BETWEEN … AND … | Whether a value is not within a range of values                          |
| STRCMP()            | Compare two strings                                                      |
| LIKE "_xxx%"        | Match exactly one character                                              |


- ****Temporary Table**** Persisted for the duration of the MySQL monitor session and removed once the session is terminated.
>
>Temporary tables can be used to work with intermediate data or results. Also, complex queries with joins or nested queries can be broken up and worked on step-by-step by storing intermediate results in temporary tables.

```sql
CREATE TEMPORARY TABLE ActorNames (FirstName CHAR(20));
```

- ****Collation & Character Sets****
>Defines what characters MySQL can store.
>- MySQL uses the **latin-1** character set that has an associated default **latin1_swedish_ci** collation.
>- The **ci** in the name implies case insensitive and the accented characters are sorted using Swedish conventions.

```sql
-- available character sets on the server
SHOW CHARACTER SET;

-- list the collations
SHOW COLLATION;

-- inspect the defaults of server
SHOW VARIABLES LIKE "c%";
```

- **NOT & XOR & STDDEV**
    - **NOT** is a unary operator that negates a boolean statement.
    - **XOR** returns true when one of the two conditions is true. False If both conditions same.
    - **STDDEV** to find disparity among records using the standard deviation function **STD**


- **ORDER BY**
    - Control the asc & desc order for each sort key.
        
        ```sql
        SELECT * FROM Actors ORDER BY NetWorthInMillions DESC, FirstName ASC;
        ```
        
    - MySQL ignores case when comparing strings in the ORDER BY clause
        - To do ASCII comparison we need to specify the **BINARY**
         keyword before the sort key.
            
            ```sql
            SELECT * FROM Actors ORDER BY BINARY FirstName;
            ```
            
    - The **CAST** function can also be used with the **ORDER BY**. The **CAST**
     function allows us to treat a column as a different type.
        
        ```sql
        SELECT * FROM Actors ORDER BY CAST(NetWorthInMillions AS CHAR);
        ```
        
- **Limit**
    - We can specify how many rows to be retrieved, starting at the offset, we specify.
        
        ```sql
        # LIMIT <offset>, <number_of_row_to_print>;
        SELECT FirstName, SecondName from Actors ORDER BY NetWorth DESC LIMIT 1000 OFFSET 3;
        ```
        
- **TRUNCATE**
    - The **TRUNCATE** statement drops a table and recreates it for faster processing.
    - **TRUNCATE** doesn’t work with locking or transactions and is the equivalent of **DELETE**
     when used with InnoDB tables.


- ****Primary Key****
    - Every time a query that contains a **WHERE** clause is issued, MySQL has to do a full scan of the table to find matching rows.
    - A linear scan of the table isn’t very efficient. Adding an *index* to the table can significantly speed-up search for matching rows.


- ****Indexes****
    - ****Index Types:****
        - ****Clustered Index****
            - Table rows are sorted and kept in a B-tree structure
            - Searching for the required data becomes easy with B-trees as they guarantee a fixed number of disk reads since they are balanced.
            - In a B+ tree, the data only lives in the leaf nodes. The root and the internal nodes contain the key on which the B+ tree is sorted.
            - A page is the smallest unit of data that a database can write to or read from a disk. A page contains rows and forms the leaf node of the B+ tree.
            - A clustered index doesn’t imply that the data rows are contiguously stored on the hard disk, only ensures that the physical and logical order the rows appear in is the same.
        - ****Non-clustered Index****
            - In a non-clustered index, the leaf-nodes don’t hold the actual data, rather, a pointer to data stored elsewhere on the disk.
            - **MyISAM** the rows aren’t stored in sorted order; Such a table is called a **heap-table** since it contains an unordered pile of data.
            - The rows appear in the insertion order when the engine is MyISAM. All indexes are secondary indexes when using MyISAM as the database engine,
    - ****Cost of Indexing****
        - It takes up additional disk space
        - It needs to be modified whenever an insert or an update is made to the table.


- **Alter**
    - For some alter operations under the hood, MySQL creates a new table with the requested alter changes, copies the data from the old table to the new one, deletes the old table, and then renames the new table to Actors.
    - An alter operation can be expensive if the table needs to be rebuilt.


- **WHERE & HAVING**
    - **WHERE** clause that can be used to filter rows.
    - **HAVING** clause allows us to filter groups.
    - Usually, the **HAVING** is used with aggregate functions. If you find yourself writing a **HAVING** clause that uses a column or expression that isn’t in the **SELECT**
     clause, it is likely you should be using the **WHERE** clause instead.


- ****Nested Queries****
    - Nested queries are generally slower but more readable and expressive than equivalent join queries.
    - **ANY | IN** match the column with *any one of*  the values returned for the SUB-QUERY.
    - **ALL** must match all the values in the group.
        
        ```sql
        SELECT FirstName FROM Actors WHERE 
        Id = ANY(SELECT ActorId FROM DigitalAssets WHERE AssetType='Facebook');
        
        SELECT FirstName FROM Actors WHERE 
        NetworthInMillions > ALL (SELECT Networth WHERE FirstName LIKE "j%");
        ```
        
    - To use **JOINs** with nested queries, MySQL requires us to provide an **alias** for the table.
        
        ```sql
        SELECT FirstName FROM Actors INNER JOIN (SELECT ActorId FROM DigitalAssets)
         AS tbl ON ActorId = Id WHERE FirstName = "Kim";
        ```
        
    
- **EXISTS |** **NOT EXISTS,**  
  - **EXISTS** operator is usually used to test if a subquery returns any rows or none at all.
        
  ```sql
  SELECT * FROM Actors WHERE EXISTS (SELECT * FROM DigitalAssets WHERE BINARY URL LIKE "%clooney%");
  ```
        

- ****Correlated Queries:**** When the inner query references a table or a column from the outer query.
    ```sql
    SELECT FirstName FROM Actors WHERE EXISTS (SELECT URL FROM DigitalAssets
    WHERE URL LIKE CONCAT("%",FirstName,"%") 
    AND AssetType="Twitter");
    ```
    -  **EXISTS** operator been used as we needed check for a condition that is true or false. No interested in what the inner query returns.


- **INSERT with SELECT** To insert several rows from another table into an existing table using a combination of select and insert statements.
    ```sql
    CREATE TABLE MyTempTable SELECT * FROM Actors;
    ```
- ****REPLACE**** like the **INSERT** but allows us the convenience of adding a row with the same primary key as an existing row in the table. 
    ```sql
    REPLACE INTO  Actors SET id = (SELECT Id FROM Actors WHERE FirstName="Brad");
    ```