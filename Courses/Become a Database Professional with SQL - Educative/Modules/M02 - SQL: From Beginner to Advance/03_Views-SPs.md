# Views And Stored Procedures

## Views

- Views are virtual tables that are created as a result of a SELECT query.
- A view can be created from a single table, by joining two tables, or from another view.

```sql
-- Create or replace a view
CREATE OR REPLACE VIEW ActorsTwitterAccounts AS
SELECT CONCAT(FirstName, ' ', SecondName) AS ActorName, URL
FROM Actors
INNER JOIN DigitalAssets  
ON Actors.Id = DigitalAssets.ActorID 
WHERE AssetType = 'Twitter';

-- Create a view from another view
CREATE VIEW RichFemaleActors AS
SELECT * FROM ActorsTwitterAccounts
WHERE Gender = 'Female';

-- Query from the view
SELECT ActorsTwitterAccounts, Age, NetWorthInMillions 
FROM ActorDetails ORDER BY Age DESC;
```

****Updatable Views****

- Used to update data in the underlying tables.
- Non Updatable View IF the create view has :
    - Aggregate functions (MAX, MIN, COUNT, SUM, etc.)
    - DISTINCT keyword
    - LEFT JOIN
    - GROUP BY, HAVING, and UNION clauses

```sql
-- Update
UPDATE ActorView SET NetWorthInMillions = 250 WHERE Id =1;

-- Insert
INSERT INTO ActorView (FirstName, SecondName, DoB, Gender, MaritalStatus,  NetWorthInMillions) 
VALUES ('Tom', 'Hanks', '1956-07-09', 'Male', 'Married', 350);

-- Delete
DELETE FROM ActorView WHERE Id = 11;
```

- To find out which views are updatable :
    
    ```sql
    SELECT Table_name, is_updatable FROM information_schema.views
    WHERE table_schema = 'mySchema';
    ```
    

****WITH CHECK OPTION****

- This clause forbids the user to insert or update rows that are not visible through the view.

```sql
CREATE OR REPLACE VIEW SingleActors AS 
SELECT FirstName, SecondName, DoB, Gender, MaritalStatus, NetWorthInMillions 
FROM Actors WHERE MaritalStatus = 'Single' 
WITH CHECK OPTION;
```

Try To Insert a married actor :

```sql
INSERT INTO SingleActors (FirstName, SecondName, DoB, Gender, MaritalStatus, NetWorthInMillions) 
VALUES ('Matt', 'Damon', '1970-10-08', 'Male', 'Married', 160);
```

Will encounter the CHECK OPTION failed error message.  as  CHECK OPTION clause ensures that only actors who are single can be inserted into the Actors table through this view.

****LOCAL AND CASCADED CHECK****

Local and cascaded check clauses are used to determine the scope of rule testing when a view is created based on another view.

- **CASCADED CHECK**
    - Means that insert or update through current view should not only be compatible with this view but also the underlying view etc …
    - Even if the upper doesn’t have CHECK OPTION still fail as **CASCADED** check option checks the rules of all underlying views before an update is allowed.
- ****Local Check → To limit the scope of rule checking****

## ****Stored Procedures****

- Procedures are a simple way to save queries for later access. A stored procedure can be defined as a set of SQL statements stored in the MySQL server.
- When the stored procedures are called
    - MySQL compiles the code of the procedure and stores it in the cache. Then the code is executed.
    - Any subsequent calls to the same stored procedure result in execution of the previously compiled code.
- **SP Advantages**
    - **Reduce the traffic** between applications and server because instead of sending queries, only the name of the stored procedure is sent to the server.
    - Since a stored procedure can be called by other procedures, it r**educes the duplication** of code.
    - **Performance gains** are also achieved by using store procedures because the code can be **pre-compiled** instead of parsing the query every time.
    - Used for **security** by giving users access to certain procedures without giving access to the tables.
- **Limitations**
    - **Difficult to debug** as they get automatically executed and it is not possible to apply checkpoints in the code.
    - Incur a resource overhead and may result in **overuse of memory and CPU.**
    - **No way to rollback** a change to a stored procedure, once it is made.

- ****Create, Alter, and Delete a Stored Procedure****
    
    ```sql
    -- create a stored procedure
    DELIMITER **
    CREATE PROCEDURE ShowActors()
    BEGIN
        SELECT *  FROM Actors;
    END **
    DELIMITER ;
    
    -- CALL statement
    CALL ShowActors();
    
    -- SHOW PROCEDURE STATUS
    SHOW PROCEDURE STATUS;
    
    -- view the stored procedures of any database
    SHOW PROCEDURE STATUS WHERE db = 'MovieIndustry';
    
    -- query the routines table in this database
    SELECT routine_name
    FROM information_schema.routines
    WHERE routine_type = 'PROCEDURE'
        AND routine_schema = 'sys';
    
    -- delete the stored procedure      
    DROP PROCEDURE IF EXISTS ShowActors;
    ```
    
- ****Variables****
    - Variables help store a user-defined, temporary value.
    - Declared before it can be used in the code. **DECLARE**
     keyword is used to declare variables by providing the data type and an optional default value.
    - Assignment of a value to a variable :
        - **SET** keyword
        - **SELECT INTO** statement.
    - The scope of a variable is local meaning that it will not be accessible after the **END** statement.
    - Inside the stored procedure, the scope of a variable depends on where it is declared.
    
    ```sql
    -- Query 1
    DELIMITER **
    CREATE PROCEDURE Summary()
    BEGIN
    	DECLARE TotalM, TotalF INT DEFAULT 0;
        DECLARE AvgNetWorth DEC(6,2) DEFAULT 0.0;
        
        SELECT COUNT(*) INTO TotalM
        FROM Actors
        WHERE Gender = 'Male';
        
        SELECT COUNT(*) INTO TotalF
        FROM Actors
        WHERE Gender = 'Female';
        
        SELECT AVG(NetWorthInMillions) INTO AvgNetWorth
        FROM Actors;
        
        SELECT TotalM, TotalF, AvgNetWorth;
    END**
    DELIMITER ;
    
    -- Query 2
    CALL Summary();
    ```
    
- ****Parameters****
    - A parameter is a placeholder for a variable that is used to pass data to and from a stored procedure.
    - Parameters are used to make a stored procedure flexible. And to avoid direct user inputs in a query string which can result in a runtime error or in worst case a malicious.
    - **Parameter Modes:**
        - **IN** → App calling the SP has to pass an argument.
        - **OUT** → The SP will pass an argument back to the caller with initial value NULL.
        - **INOUT** → The caller may pass an argument to the SP and the SP can work on it and pass the altered value back to the caller.
    
    ```sql
    -- OUT to pass argument back
    DELIMITER **
    CREATE PROCEDURE GetActorCountByNetWorth (
    	IN  NetWorth INT,
    	OUT ActorCount INT)
    BEGIN
    	SELECT COUNT(*) INTO ActorCount
    	FROM Actors
    	WHERE NetWorthInMillions >= NetWorth;
    END**
    DELIMITER ;
    
    -- CALL
    CALL GetActorCountByNetWorth(500, @ActorCount);
    SELECT @ActorCount;
    
    -- INOUT to access original and maodify it
    DELIMITER **
    CREATE PROCEDURE IncreaseInNetWorth(
    	INOUT Increase INT,
    	IN ActorId INT )
    BEGIN
    	DECLARE OriginalNetWorth INT;
    
    	SELECT NetWorthInMillions Into OriginalNetWorth
        FROM Actors 
    	WHERE Id = ActorId;
    
    	SET Increase = OriginalNetWorth + Increase;	
    END**
    DELIMITER ;
    
    --CALL
    SET @IncreasedWorth = 50;
    CALL IncreaseInNetWorth(@IncreasedWorth, 11);
    SELECT @IncreasedWorth;
    ```
    
- ****Conditional Statements****
    - **IF**
    
    ```sql
    DROP PROCEDURE GetMaritalStatus;
    DELIMITER **
    CREATE PROCEDURE GetMaritalStatus(
        IN  ActorId INT, 
        OUT ActorStatus  VARCHAR(30))
    BEGIN
        DECLARE Status VARCHAR (15);
    
        SELECT MaritalStatus INTO Status
        FROM Actors
        WHERE Id = ActorId;
    
        IF Status LIKE 'Married' THEN
            SET ActorStatus = 'Actor is married';
        ELSEIF Status LIKE 'Single' THEN
            SET ActorStatus = 'Actor is single';
        ELSEIF Status LIKE 'Divorced' THEN
            SET ActorStatus = 'Actor is divorced';
        ELSE
            SET ActorStatus = 'Status not found';
        END IF;
    END **
    DELIMITER ;
    ```
    
    - **CASE**
    
    ```sql
    DROP PROCEDURE GetMaritalStatus;
    
    DELIMITER **
    CREATE PROCEDURE GetMaritalStatus(
     IN  ActorId INT, 
     OUT ActorStatus VARCHAR(30))
    BEGIN
     DECLARE Status VARCHAR (15);
    
     SELECT MaritalStatus INTO Status
     FROM Actors 
     WHERE Id = ActorId;
    
     CASE Status
         WHEN 'Married' THEN
             SET ActorStatus = 'Actor is married';
         WHEN 'Single' THEN
             SET ActorStatus = 'Actor is single';
         WHEN 'Divorced' THEN
             SET ActorStatus = 'Actor is divorced';
         ELSE
             SET ActorStatus = 'Status not found';
     END CASE;
    END**
    
    DELIMITER ;
    ```
    
- ****Iterative Statements****
    - ****LOOP →**** Any statements between the **LOOP**
     and **END LOOP**
     keywords are repeated until a condition for termination is met.
        - The **LEAVE** statement is used to break the iterative processing.
        - **ITERATE** statement is used to ignore processing and start a new iteration of the loop.
        
        ```sql
        Print_loop: LOOP
         IF CurrentRow > TotalRows THEN
           LEAVE Print_loop;
         END IF;
        
        IF gen NOT LIKE 'Male' THEN
         SET CurrentRow = CurrentRow + 1;
         ITERATE Print_loop;
        ELSE
         SELECT FirstName INTO fname 
         FROM Actors 
         WHERE Id = CurrentRow;
        
        END IF;
        END LOOP Print_loop;
        ```

    - **WHILE**
        
        ```sql
        [label] WHILE condition DO
        
        statements;
        
        END WHILE [label]
        ```
        
        ```sql
        Print_loop: WHILE CurrentRow < TotalRows DO
         SELECT Gender INTO gen 
         FROM Actors 
         WHERE Id = CurrentRow;
        
         IF gen LIKE 'Male' THEN
           SELECT FirstName INTO fname 
           FROM Actors 
           WHERE Id = CurrentRow;
        
           SELECT SecondName INTO lname 
           FROM Actors 
           WHERE Id = CurrentRow;
        
           SET  str = CONCAT(str,fname,' ',lname,', ');
         END IF;
        ```

    - ****REPEAT****
        
        ```sql
        [label:] REPEAT
        
        statements;
        
        UNTIL condition
        
        END REPEAT [label]
        ```
        
        ```sql
        Print_loop: REPEAT 
         SELECT Gender INTO gen 
         FROM Actors 
         WHERE Id = CurrentRow;
        
         IF gen LIKE 'Male' THEN
           SELECT FirstName INTO fname 
           FROM Actors 
           WHERE Id = CurrentRow;
        
           SELECT SecondName INTO lname 
           FROM Actors 
           WHERE Id = CurrentRow;
        
           SET  str = CONCAT(str,fname,' ',lname,', ');
         END IF;
         
         SET CurrentRow = CurrentRow + 1;
         UNTIL CurrentRow > TotalRows
         END REPEAT Print_loop;
        ```
- Stored Functions
    
    
|  | Stored Procedure | Stored Function |
| --- | --- | --- |
| Call the other | Can call SF | Not possible |
| Inline SQL Statements | Only by CALL keyword | can be used in SQL statements |
| Compile State | Pre-compiled and Stored | Compiled at Runtime |
| Return value | Optional | Must |
| # Return values | no restriction | one value |
| Params modes | IN, OUT, INOUT | Only IN |
- Data type of the return value is specified after the **RETURN**
 keyword.
- The function body must have at least one **RETURN**
 statement.
- **DETERMINISTIC** or **NOT DETERMINISTIC** → for the same input parameters, the result will either be the same or different.
    
    ```sql
    DELIMITER **
    
    CREATE FUNCTION function_name(parameter_list)
    
    RETURNS datatype
    
    [NOT] DETERMINISTIC
    
    BEGIN
    
    function body
    
    END **
    
    DELIMITER ;
    ```
    
    ```sql
    DELIMITER **
    
    CREATE FUNCTION DigitalAssetCount(ID INT) 
    RETURNS VARCHAR(50)
    DETERMINISTIC
    BEGIN
     DECLARE ReturnMessage VARCHAR(50);
     DECLARE Number INT DEFAULT 0;
     SELECT COUNT(*) INTO Number FROM DigitalAssets WHERE ActorId = ID;
    
     IF Number = 0 THEN
         SET ReturnMessage = 'The Actor does not have any digital assets.';
     ELSE
         SET ReturnMessage = CONCAT('The Actor has ', Number, ' digital assets');
     END IF;
     
     -- return the customer level
     RETURN (ReturnMessage);
    END**
    DELIMITER
    ```
    
    ```sql
    SELECT Id, DigitalAssetCount(Id) AS Count FROM Actors;
    ```