# Clustered Index
```
- Determine the physical order of data in a table even if we insert data id not in a sequential order
- Each table has one clustered index
- In sql server , table always has clustered index default is Primary KEY
- One clustered index can have multiple column => Composite clustered index
```
`CREATE CLUSTERED INDEX IX_Person_Id ON Person(Id)`

Multiple column
`CREATE CLUSTERED INDEX IX_Person_Gender_Salary ON Person(Gender ASC, Salary DESC)`
=> Table will be sort by gender and then Salary

## why Clustered index make query faster , not just sort data
SQL server read PAGE (default is 8KB)
```
Table
 ├─ Page 1 (8KB)
 ├─ Page 2 (8KB)
 ├─ Page 3 (8KB)
```
1 page can container multiple row
