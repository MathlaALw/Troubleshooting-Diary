# Troubleshooting Diary

### Error 1:

Msg 195, Level 15, State 10, Procedure GetStudentDeptName, Line 11 [Batch Start Line 30]
'GetStudentDeptName' is not a recognized built-in function name.

### Solution:
```sql

CREATE FUNCTION dbo.GetStudentDeptartmentName (@StudentId INT)
RETURNS VARCHAR(100)
AS
BEGIN
    DECLARE @DeptName VARCHAR(100);

    SELECT @DeptName = d.Dept_Name
    FROM Student s
    JOIN Department d ON s.Dept_Id = d.Dept_Id
    WHERE s.St_Id = @StudentId;

    RETURN @DeptName;
END;

--------- call function ---

select * from Student;

-> check if the function exists

SELECT OBJECT_NAME(object_id), type_desc 
FROM sys.objects 
WHERE name = 'GetStudentDeptartmentName';

-> call the function

SELECT dbo.GetStudentDeptartmentName(1)

```
------------------

### Error 2:  Restore the Database from Backup Files task (Drop the Current Database) 

Msg 3702, Level 16, State 4, Line 149
Cannot drop database "TrainingDB" because it is currently in use.


### Solution:

#### Set the database to single-user mode and rollback active connections
 
```sql

ALTER DATABASE TrainingDB SET SINGLE_USER WITH ROLLBACK IMMEDIATE;

```

#### Then ,Now drop the database
```sql
DROP DATABASE TrainingDB;


```

----------------

### Error 3: ViewAvailableBooks  ->  Available books grouped by genre, ordered by price - FROM LIBRARY PROJECT PART 2 - VIEW.SQL FILE


Msg 1033, Level 15, State 1, Procedure ViewAvailableBooks, Line 5 [Batch Start Line 27]
The ORDER BY clause is invalid in views, inline functions, derived tables, subqueries, and common table expressions, unless TOP, OFFSET or FOR XML is also specified.

### Solution:
```sql

CREATE VIEW ViewAvailableBooks AS
SELECT Genre, Title, Shelf_Location,Price
FROM Book
WHERE Available_State = 'TRUE'



```
**Then:**
```sql

SELECT * FROM ViewAvailableBooks
ORDER BY Price , Genre;

```
|
~~NOTE:~~
**SQL Server** does not allow ORDER BY in a view unless you use it with TOP, OFFSET, or FOR XML.

-----


### Error 4: ViewAvailableBooks  ->  Available books grouped by genre, ordered by price - FROM LIBRARY PROJECT PART 2 - VIEW.SQL FILE

Msg 8120, Level 16, State 1, Procedure ViewAvailableBooks, Line 3 [Batch Start Line 26]
Column 'Book.Title' is invalid in the select list because it is not contained in either an aggregate function or the GROUP BY clause.
**PROBLEM CODE:**
```sql
CREATE VIEW ViewAvailableBooks AS
SELECT Genre,Title,Price,Shelf_Location FROM Book
WHERE Available_State = 'TRUE'
GROUP BY Genre

SELECT * FROM ViewAvailableBooks
ORDER BY Price , Genre;
```

