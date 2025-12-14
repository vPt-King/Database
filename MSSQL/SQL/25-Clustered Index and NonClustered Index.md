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

## NonClustered Index
Nonclustered index = một cấu trúc dữ liệu RIÊNG, chứa giá trị của cột được index + con trỏ (row locator) trỏ về dòng dữ liệu thật
📌 Nó không chứa toàn bộ dòng dữ liệu (trừ cột INCLUDE).
Example:
```
Users(
    Id INT PRIMARY KEY CLUSTERED,
    Email VARCHAR(100),
    Name VARCHAR(50),
    Age INT
)
```
Tạo index:
```
CREATE NONCLUSTERED INDEX IX_Users_Email
ON Users(Email);
```
🔹 Bên trong nonclustered index
```
Email               → Row Locator
--------------------------------
a@gmail.com         → Id = 1
b@gmail.com         → Id = 5
c@gmail.com         → Id = 20
```
📌 Row Locator = clustered key (Id)
(nếu bảng là HEAP thì là RID)

## So sánh
+ Clustered Index thì nhanh hơn một chút so với NonClustered index do không phải reference đến index mà bản thân bảng được đánh clustered index là B-tree

+ 1 table chỉ có 1 clustered index và có thể có 1 hoặc nhiều nonclustered index

+ NonClustered table require more diskspace