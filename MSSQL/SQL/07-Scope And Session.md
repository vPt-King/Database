# Scope and session
+ Session
A session is the connection between a client (like SSMS, an application, or a script) and SQL Server.
Each time you connect to SQL Server, you start a new session.
Multiple batches, procedures, or triggers can run inside the same session.
+ Scope
A scope is a boundary inside SQL Server where variables and identity values are valid.
Examples of scopes:
A batch (GO separates batches)
A stored procedure
A trigger
Identity values generated inside a scope are tracked separately.