```
3 types of trigger in SQL server
1. DML triggers
2. DDL triggers
3. Logon triggers
``` 
# DML Triggers
`DML triggers are fired automatically in response to DML events (Inserts, update & Delete)`

DML triggers can be again classified into 2 types
```
1. After triggers (Some time call FOR triggers)
2. Instead of triggers
```

## After triggers
Fires after the triggering action. The insert, update, delete statement causes an after trigger to fire after the respective statments complete execution.

Example 1
```
CREATE TRIGGER tr_tblEmployee_ForInsert
ON tblEmployee
FOR INSERT
AS
BEGIN
    DECLARE @Id int
    Select @Id = Id from inserted
    insert into tblEmployeeAudit values('New Employee with id = ' + Cast(@Id as nvarchar(5)) + ' is added at ' + cast(Getdate() as nvarchar(20)))
END
```
Example 2:
```
CREATE TRIGGER tr_tblEmoloyee_ForDelete
ON tblEmployee
AS
BEGIN
    Declare @Id int
    Select @Id = Id from deleted
    insert into tblEmployeeAudit values('An existing employee with Id = ' + Cast(@Id as nvarchar(5)) + ' is deleted at ' + Cast(Getdate() as nvarchar(20)))
END
```

### Inserted and deleted table
```
Inserted and deleted is just virtual tables only existed in trigger
inserted : contant data after inserted / updated
deleted : contant data before deleted / updated
```

### Warning
Need to write code like this:
```
INSERT INTO SalaryLog (EmpId, OldSalary, NewSalary)
SELECT d.Id, d.Salary, i.Salary
FROM deleted d
JOIN inserted i ON d.Id = i.Id;
```

## Instead of triggers
fires instead of the trigggering action. The insert, upate and delte statments cause an instead of trigger to fire instead of the respsective statment execution.

When to use:
```
When we insert data into a view (the real data is base on multiple tables) , SQL server will thow an error that it will affects multiple base tables.
We will create a triggers to insert real data into base tables
```

example 1:
```
CREATE TRIGGER tr_vEmployeeDetails_InsteadOfInsert
on vEmployeeDetails
Instead of Insert
AS 
BEGIN
    Declare @DeptId int
    --check if there is a balid DepartmentID
    --for the give DepartmentName
    Select @DeptId = DeptId from tblDepartment
    join inserted
    on inserted.DeptName = tblDepartment.DeptName

    --If department Id is null then we throw an error
    --and stop processing
    if(@DeptId is null)
    BEGIN
        Raiserror('Invalid department name, statment terminate', 16,1)
        return
    END
    Insert into tblEmployee(Id, Name, Gender, DepartmentId)
    Select Id, name, Gender, @DeptId from inserted
END
```
