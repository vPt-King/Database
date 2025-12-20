
# Identity column
We can check if one column having property identity or not
Primary key is not identity column

Identity property is an auto-increment 
```
Seed : start value
Increment : incremental value
 Example : ID INT IDENTITY(1,1) PRIMARY KEY
```

1. Reset identity
`DBCC CHECKIDENT ('{table_name}', RESEED, 0)`

2. Get identity column value
Method 1: SCOPE_IDENTITY() build in function (Recommended ✅)
Returns the last identity value generated in the same scope (same session and same stored procedure/trigger).
Safest option because it avoids interference from triggers or other inserts happening in parallel.
```
INSERT INTO Employees (Name, Position) VALUES ('Alice', 'Manager');
SELECT SCOPE_IDENTITY() AS LastIdentity;
```
Method 2: @@Identity
Returns the last identity value generated in the current session, but across all scopes.
Can be problematic if a trigger inserts into another table with an identity column, because it will return that value instead.
```
INSERT INTO Employees (Name, Position) VALUES ('Bob', 'Developer');
SELECT @@IDENTITY AS LastIdentity;
```

Method 3: IDENT_CURRENT(‘{table_name}’)
Returns the last identity value generated for a specific table, regardless of session or scope.
Useful if you want the identity value from a particular table, but not necessarily from your own insert.
```
SELECT IDENT_CURRENT('Employees') AS LastIdentity;
```
Use SCOPE_IDENTITY() when you need the identity value from your own insert.
Use IDENT_CURRENT('TableName') if you need the last identity value from a specific table, regardless of who inserted it.