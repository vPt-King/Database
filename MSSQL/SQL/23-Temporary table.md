# Temporary Tables
Like created by TempDB and will be auto deleted when no need use
Two type of Temporary tables
+ Local tables
+ Global Tables

## Local Temporary Tables
```
- Exists only in a connected session
- Automatically deleted when session is closed
- Only connection that created it can see local temporary tables
- Stored in System database -> tempdb
- Temporary table in Procedure will be deleted after procedure is executed
- Possible for different connections to create a local temporary table with the same name
- SQL server create a random number and suffix it to temporary table name
```

To created temporary tables we prefix the name of tables with '#'
`Select * from #Person;`

Check if the local temporary table is created: 
`SELECT name FROM tempdb..sysobjects where name like '#...%'`

If User wants to explicitly drop the temporary table , he can do so using DROP command
`DROP TABLE $PersonDetail`

## Global temporary tables
```
- Every connection can see global temporary tables
- Only is deleted when last session that reference to it is closed or SQL server restart
- It is not possible for different user can create a same temporary table name
- SQL server wil not create a random number and suffix it to temporary table name
```

To create a temporary table we prefix the name of table with '##'
`Create table ##PersonDetails(Id int, Name nvarchar(20));`

## When to use Local Temporary table and Global temporary table
In SQL Server, choosing between Local Temporary Tables (#) and Global Temporary Tables (##) depends on scope, concurrency, and lifetime.

For Local temporary table:
```
-Data is only needed within one session
-Used inside a stored procedure or single script
-Multiple users may run the same code at the same time
-You want automatic cleanup when the session ends
-You need indexes, statistics, or large data processing

Typical scenarios
Stored procedures with complex logic
Breaking large queries into steps
Replacing nested subqueries
Per-user temporary calculations
Web applications / APIs
```
For Global temporary table:
```
Use Global Temporary Tables when:

- Data must be shared across multiple sessions
- Used by multiple SQL Agent job steps
- One session creates data, others consume it
- Short-lived system-level scripts
- NOT for high-concurrency applications

Typical scenarios
SQL Agent Jobs (step 1 create → step 2 read)
Maintenance / migration scripts
Reporting scripts shared by admins
One-time data synchronization
```


