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

