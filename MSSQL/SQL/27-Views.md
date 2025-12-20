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

## Updatable views
We can insert , update, delete data using view BUT
View must reference one table
No GROUP BY, DISTINCT, JOIN, aggregate functions
```
Update [viewname] ...
Delete from [viewname] ...
Insert into [viewname] ...
```

## Indexed View
When we create an index on a view , the view gets materialized this mean view now can store data
Rule:
```
1. Must using with SCHEMABINDING option
2. if value is NULL, replace it (12-Replace NULL.md)
3. if GROUP BY is specified, the view select list must contain a COUNT_BIG(*) expression
COUNT_BIG(*) is an aggregate function in SQL Server that works like COUNT(*), but returns a BIGINT value instead of INT.
4. The base tables in the view, should be referenced with 2 part name
```

Create index :
`CREATE unique clusterd [rule role] on [tablename] (column)`


## View limitation
```
1. You cannot pass parameters to a view 
2. Rules and Defaults cannot be associated with views
3. The order by clause is invalide in views unless TOP or FOR XML is also specified
4. Views cannot be based on temporary tables
``` 
## When to using view
```
- When you don't want grant access the whole table. instead you should create a view for user

- Reuse query 
```