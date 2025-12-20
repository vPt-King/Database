# Views
A view is nothing more than a saved SQL. A view ca also be considered as a virtual table

## Create view
```
CREATE VIEW [VIEWNAME]
AS
[QUERY]
```

We can retrive data from view like a table
`Select * from [viewname]`

View does not contain any real data

Example:
```
CREATE VIEW vw_EmployeeInfo
AS
SELECT Id, Name, Department, Salary
FROM Employees
WHERE IsActive = 1;

SELECT * FROM vw_EmployeeInfo;
```


## Advantages of views
```
- Views can be used to reduce the complexity of the data schema

- Views can be used as a mechanism to implement row and column level security

- Views can be used to present aggredated data and hide detail data
```

## When to using view
```
- When you don't want grant access the whole table. instead you should create a view for user

- Reuse query 
```