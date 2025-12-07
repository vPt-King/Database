6, Default constraint

6.1 Adding default constraint to a exist table
```
alter table {TABLE_NAME} ADD CONSTRAINT CONSTRAINT_NAME DEFAULT {DEFAULT_VALUE} for {column_name}
```

6.2  Dropping constraint
`alter table {TABLE_NAME} drop constraint {CONSTRAINT_NAME}`


7, Cascading referential integrity constraint
This is allows to define the actions of SQL server should take when a row which is existed foreign key is updated or deleted
By default an error will occur if this happen

Option when setting up Cascading referential integrity constraint:
7.1 No action
This is the default behaviour. No action specifies that if an attempt is made to delete or update row with a  key referenced by foreign keys in existing rows in other tables, an error is raised and the delete or update is rollback

7.2 Cascade 
Specifies that if an attempt is made to delete or update a row with a key referenced by foreign key in existing row in other tables , all rows containing those foreign keys are also deleted or updated.
7.3 Set Null
Set null if a key is deleted or updated

7.4 Set Default
Default values is set when a key is deleted or updated

7.5 Command
```
ALTER TABLE <ten_bang_con>
ADD CONSTRAINT <ten_constraint>
FOREIGN KEY (<cot_fk>)
REFERENCES <ten_bang_cha>(<cot_pk>)
ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT }
ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT };
```

Remember: FK ở bảng con được set về giá trị default của cột đó

8, Check constraint 
Check constraint is responsible for checking the value which is updated to the column

If the value updated to the row does not satisfied the boolean expression, then it will not allow to insert to the column

8.1 Add check constraint 
```
Alter table {table_name} add constraint {constraint_name} check {boolean_expression}

alter table tblPerson add constraint ck_person_age check (AGE > 0 and AGE < 150 )
```

8.2 Drop check constraint 
```
Alter table {table_name} drop constraint {constraint_name}
```

9, Identity column
We can check if one column having property identity or not
Primary key is not identity column

Identity property is an auto-increment 
```
Seed : start value
Increment : incremental value
 Example : ID INT IDENTITY(1,1) PRIMARY KEY
```

9.1 Reset identity
`DBCC CHECKIDENT ('{table_name}', RESEED, 0)`

9.2 Get identity column value
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


10, Scope and session
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


11, Unique key constraint
Unique is mean no duplicate in column

`alter table {table_name} add constraint uq_{table_name}{column} unique({column_name})`


1 table have one primary key and can have more than 2 unique key
primary key not allow to contain null value

12. Select 
13. Group by
GROUP BY trong SQL Server dùng để gom nhóm các dòng có giá trị giống nhau trong một hoặc nhiều cột, sau đó bạn có thể dùng hàm tổng hợp (aggregate functions) để tính toán trên từng nhóm.
Gom nhóm dữ liệu theo một hoặc nhiều cột


Kết hợp với hàm tổng hợp để tính:


COUNT() – đếm số dòng


SUM() – tính tổng


AVG() – tính trung bình


MAX() – giá trị lớn nhất


MIN() – giá trị nhỏ nhất

```
SELECT column1, column2, aggregate_function(column3)
FROM table_name
GROUP BY column1, column2;
```
Có thể kết hợp sử dụng where và having
where thì đứng trước group by là lọc trước khi gộp
having thì lọc sau khi gộp và viết sau group by

14, Schema
Schema trong SQL Server là một “nhóm” (namespace) dùng để chứa các đối tượng trong database, như:
Table (bảng)
View
Stored Procedure
Function
Trigger
Synonym
Type


Nó giống như folder hoặc namespace để tổ chức các đối tượng trong cơ sở dữ liệu.
14.1, Định nghĩa chính xác
Schema = một container logic để quản lý và phân quyền các object trong database.
Ví dụ:
dbo.Customers
sales.Orders
hr.Employees
report.DailyRevenue
Schema đứng trước tên bảng, phân biệt các đối tượng có cùng tên nhưng khác schema.

14.2, Tại sao lại cần schema
Database lớn có thể có hàng trăm bảng → chia theo schema sẽ dễ quản lý:
dbo → bảng hệ thống chính
sales → nghiệp vụ bán hàng
hr → nhân sự
audit → bảng log
report → các view báo cáo

14.3, Phân quyền
Bạn có thể cấp quyền theo schema, không cần cấp từng bảng:
GRANT SELECT ON SCHEMA::sales TO userA;

User A chỉ xem được bảng trong schema sales.

14.4, Schema mặc định
Schema mặc định là dbo.
Nghĩa là khi bạn tạo bảng:
CREATE TABLE Customers (...);

Nó thật ra là:
CREATE TABLE dbo.Customers (...);
14.5, Tạo schema mới
CREATE SCHEMA sales;
GO

CREATE TABLE sales.Orders (
    OrderID INT,
    Amount DECIMAL(10,2)
);


14.6, Chuyển 1 table sang 1 schema khác
`ALTER SCHEMA sales TRANSFER dbo.Customers;`

Schema là 1 namespace trong database dùng để:
Tổ chức các bảng và đối tượng
Phân quyền dễ dàng
Tránh trùng tên object
Một database có thể có nhiều schema
Schema quan trọng như thư mục trong filesystem vậy.

14.7, Kiểm tra schema trong database
`SELECT name  FROM sys.schemas;`

```
SELECT 
    s.name AS SchemaName,
    u.name AS OwnerName
FROM sys.schemas s
JOIN sys.sysusers u ON u.uid = s.principal_id;
```
```
SELECT * 
FROM INFORMATION_SCHEMA.SCHEMATA;
```
14.8, Mối quan hệ User và Schema
 SQL Server: User và Schema là hai thứ tách biệt
User = tài khoản để đăng nhập, phân quyền
Schema = nơi chứa các object (bảng, view, proc…)
Một user có thể dùng nhiều schema, và nhiều user có thể dùng chung một schema.
Vậy user lưu object ở đâu?
Mặc định, nếu user không có schema riêng thì SQL Server sẽ dùng schema dbo.
Ví dụ:
CREATE TABLE Users (...);

→ Thực tế là tạo trong:
dbo.Users
 Nếu muốn user có schema riêng thì sao?
Bạn phải tự tạo schema, không tự động như Oracle.
Ví dụ tạo schema theo tên user:
CREATE SCHEMA thanh AUTHORIZATION thanh;

Rồi tạo bảng:
CREATE TABLE thanh.Customers (...);

15,  Join
15.1 Inner join
Chỉ trả về phần giao nhau.
Ví dụ:
SELECT *
FROM A
INNER JOIN B ON A.id = B.a_id;

15.2 Left join
Lấy tất cả dữ liệu của bảng bên trái, và những dòng khớp ở bảng phải
 → Nếu bảng phải không có thì trả NULL.
Ví dụ:
SELECT *
FROM A
LEFT JOIN B ON A.id = B.a_id;

15.3 Right join
Ngược lại với LEFT JOIN.
 Lấy tất cả dữ liệu của bảng bên phải, và dữ liệu khớp ở bảng trái.
Ví dụ:
SELECT *
FROM A
RIGHT JOIN B ON A.id = B.a_id;

15.4 Full join
Lấy tất cả dữ liệu của cả 2 bảng,
 Dòng nào không khớp thì trả NULL ở phía còn lại.
Ví dụ:
SELECT *
FROM A
FULL OUTER JOIN B ON A.id = B.a_id;

15.5 Cross join
Nhân Cartesian – mỗi dòng của bảng A kết hợp với mỗi dòng của bảng B.
Ví dụ:
 A có 3 dòng, B có 4 dòng → kết quả 12 dòng.
SELECT *
FROM A
CROSS JOIN B;

15.6 Self join
JOIN chính nó (thường để xử lý dữ liệu dạng cây – parent/child).
Ví dụ (lấy nhân viên + quản lý của họ):
SELECT e.Name AS Employee, m.Name AS Manager
FROM Employee e
LEFT JOIN Employee m ON e.ManagerId = m.Id;

16, Advance join
16.1 Retrive not maching left join
LEFT JOIN + lọc những dòng không match bằng cách check NULL của cột trong điều kiện on ở bảng bên phải.
Công thức chung:
```
SELECT *
FROM A
LEFT JOIN B ON A.key = B.key
WHERE B.key IS NULL;
```
16.2 Retrive not maching right join
RIGHT JOIN + lọc những dòng không match bằng cách check NULL của cột trong điều kiện on ở bảng bên trái.
Công thức chung:
```
SELECT *
FROM A
RIGHT JOIN B ON A.key = B.key
WHERE A.key IS NULL;
```

16.3 Retrive not match inner join

Công thức chung:
```
SELECT *
FROM A
RIGHT JOIN B ON A.key = B.key
WHERE A.key IS NULL or B.key is NULL;
```

17, Replace NULL in SQL server
17.1 ISNULL function

```
select E.Name as Employee , ISNULL(M.Name, ‘No manager’) as Manager 
from tblEmployee E
left join tblEmployee M
on E.ManagerID = M.EmployeeID
```

17.2 CASE Statement
```
select E.Name as Employee , 
case 
	when M.Name is NULL then ‘NO MANAGER’ else M.Name
end
as M.Manager
from tblEmployee E
left join tblEmployee M on E.ManagerID = M.EmployeeID
```

17.3 COALESCE function
COALESCE trong SQL Server là hàm trả về giá trị không NULL đầu tiên trong danh sách các biểu thức.
Hiểu đơn giản:
COALESCE(a, b, c, …) → trả về giá trị đầu tiên khác NULL từ trái sang phải.

```
select E.Name as Employee , COALESCE(M.Name, ‘No manager’) as Manager 
from tblEmployee E
left join tblEmployee M
on E.ManagerID = M.EmployeeID
```

18. Union và Union ALL
UNION trong SQL là phép kết hợp kết quả của hai hoặc nhiều SELECT lại với nhau thành một tập kết quả duy nhất.
Nó hoạt động như phép “gộp danh sách” nhưng loại bỏ các dòng trùng nhau.
SELECT Name FROM Students_A
UNION
SELECT Name FROM Students_B;
SQL sẽ trả về:

Tất cả tên từ bảng A

Tất cả tên từ bảng B

Không có dòng trùng nhau (ví dụ trùng tên sẽ chỉ xuất hiện 1 lần)


Union ALL sẽ giữ nguyên tất cả các dòng của cả 2 câu lệnh
Yêu cầu khi dùng UNION
Số cột phải bằng nhau


Kiểu dữ liệu tương ứng phải tương thích
19. Stored Procedure
Stored Procedure trong SQL Server là một khối lệnh SQL (nhiều câu lệnh) được lưu sẵn trong database để tái sử dụng như một “hàm” trong lập trình.
Hiểu đơn giản:
Stored Procedure = đoạn code SQL được lưu sẵn → gọi khi cần, không phải viết lại.
19.1 Stored procedure dùng làm gì
1. Tự động hóa công việc
Ví dụ: tính lương, tạo report, backup, gửi email...
2. Giảm lặp code
Không cần viết lại nhiều câu SELECT/INSERT/UPDATE phức tạp.
3. Bảo mật tốt hơn
Chỉ cần cấp quyền “EXECUTE” → user không cần biết cấu trúc bảng.
4. Tăng hiệu suất
SQL Server compile & optimize procedure sẵn → chạy nhanh hơn.
19.2 Stored Procedure đơn giản
```
CREATE PROCEDURE GetAllUsers
AS
BEGIN
    SELECT * FROM Users;
END;

Gọi procedure
EXEC GetAllUsers;
```


19.3 Stored Procedure có tham số
```
CREATE PROCEDURE GetUserById
    @UserId INT
AS
BEGIN
    SELECT * FROM Users WHERE Id = @UserId;
END;

gọi 
EXEC GetUserById @UserId = 10;
```

19.4 Stored Procedure với INSERT/UPDATE/DELETE

```
CREATE PROCEDURE CreateUser
    @Name NVARCHAR(100),
    @Email NVARCHAR(100)
AS
BEGIN
    INSERT INTO Users (Name, Email)
    VALUES (@Name, @Email);
END;


EXEC CreateUser 'Thanh', 'thanh@example.com';
```

19.5  Stored Procedure trả về giá trị (return)
```
CREATE PROCEDURE GetTotalUsers
    @Total INT OUTPUT
AS
BEGIN
    SELECT @Total = COUNT(*) FROM Users;
END;
Gọi:

DECLARE @x INT;
EXEC GetTotalUsers @Total = @x OUTPUT;
SELECT @x AS TotalUsers;
```

20, String buil in funtion in sql server
20.1, LEN() – Độ dài chuỗi (không tính khoảng trắng cuối)
`SELECT LEN('Hello');  -- 5`

20.2, DATALENGTH() – Độ dài chuỗi tính theo byte
`SELECT DATALENGTH('Hello');  -- 5 bytes`


20.3, LOWER() / UPPER()
```
SELECT LOWER('HELLO');  -- hello
SELECT UPPER('hello');  -- HELLO
```

20.4, LEFT() / RIGHT()
```
SELECT LEFT('SQL Server', 3);   -- SQL
SELECT RIGHT('SQL Server', 6);  -- Server
```
20.5, CHARINDEX() – Tìm vị trí
`SELECT CHARINDEX('er', 'Server');  -- 4`


20.6, PATINDEX() – Tìm theo pattern (có wildcards)
`SELECT PATINDEX('%ver%', 'SQL Server'); -- 5`

20.7, SUBSTRING() – Cắt chuỗi

`SELECT SUBSTRING('SQL Server', 5, 6);  -- Server`


20.8, REPLACE() – Thay thế
`SELECT REPLACE('Hello World', 'World', 'SQL');  -- Hello SQL`


20.9, LTRIM() / RTRIM() – Xóa khoảng trắng trái/phải
```
SELECT LTRIM('   hello');  -- 'hello'
SELECT RTRIM('hello   ');  -- 'hello'
```

20.10, CONCAT() – Nối chuỗi an toàn NULL**
`SELECT CONCAT('Hello ', NULL, 'SQL');  -- Hello SQL`


20.11, CONCAT_WS() – Nối chuỗi có separator
`SELECT CONCAT_WS('-', 'A', 'B', 'C');  -- A-B-C`

20.12, REPLICATE() – Lặp chuỗi
`SELECT REPLICATE('*', 5);  -- *****`

20.13, STUFF() – Chèn chuỗi vào vị trí bất kỳ
`SELECT STUFF('Hello', 2, 3, 'i');  -- Hilo`


20.14, ASCII() / UNICODE()
```
SELECT ASCII('A');       -- 65
SELECT UNICODE(N'Đ');    -- mã unicode
```
20.15, CHAR() / NCHAR() – Trả về ký tự từ mã ASCII/Unicode
`SELECT CHAR(65);   -- 'A'`


20.16, FORMAT() – Format số/ngày thành chuỗi
`SELECT FORMAT(GETDATE(), 'dd/MM/yyyy');`


20.17, STRING_SPLIT() – Tách chuỗi thành nhiều dòng
`SELECT * FROM STRING_SPLIT('A,B,C', ',');`


20.18, STRING_AGG() – Gộp nhiều dòng thành chuỗi
`SELECT STRING_AGG(Name, ',') FROM Users;`

21.  Datetime in sql
21.1, GETDATE()
Trả về ngày giờ hiện tại (datetime).
`SELECT GETDATE();`


21.2, SYSDATETIME()
Trả về ngày giờ hiện tại, độ chính xác cao hơn (datetime2).
`SELECT SYSDATETIME();`


21.3. CURRENT_TIMESTAMP
Tương đương GETDATE(), chuẩn ANSI.
`SELECT CURRENT_TIMESTAMP;`


21.4. GETUTCDATE() / SYSUTCDATETIME()
Ngày giờ UTC.
```
SELECT GETUTCDATE();
SELECT SYSUTCDATETIME();
```

21.5, DATEADD() — Cộng/trừ thời gian
```
SELECT DATEADD(DAY, 7, GETDATE());  -- +7 ngày
SELECT DATEADD(MONTH, -1, GETDATE()); -- -1 tháng
```

21.6, DATEDIFF() — Khoảng cách 2 ngày
`SELECT DATEDIFF(DAY, '2024-01-01', '2024-01-10');  -- 9`


21.7, DATEDIFF_BIG()
Giống DATEDIFF nhưng dùng BigInt (hàng tỷ mili giây).

21.8, DATEFROMPARTS()
Tạo ngày từ năm/tháng/ngày.
`SELECT DATEFROMPARTS(2024, 12, 20);`


21.9, DATETIMEFROMPARTS()
Tạo datetime từ tất cả thành phần.

`SELECT DATETIMEFROMPARTS(2024, 12, 20, 10, 30, 0, 0);`


21.10, EOMONTH() — Lấy ngày cuối tháng
```
SELECT EOMONTH(GETDATE());              -- Cuối tháng này
SELECT EOMONTH(GETDATE(), 1);           -- Cuối tháng sau
```

21.11, DATEPART() — Lấy từng phần của ngày
```
SELECT DATEPART(YEAR, GETDATE());   -- Năm
SELECT DATEPART(MONTH, GETDATE());  -- Tháng
SELECT DATEPART(WEEKDAY, GETDATE());-- Thứ trong tuần
```

21.12, DATENAME() — Lấy tên dạng chữ
`SELECT DATENAME(WEEKDAY, GETDATE());  -- Monday, Tuesday...`

21.13, CONVERT() — Định dạng ngày (format)
```
SELECT CONVERT(VARCHAR(10), GETDATE(), 103);  -- dd/MM/yyyy
SELECT CONVERT(VARCHAR(19), GETDATE(), 120);  -- yyyy-mm-dd hh:mi:ss

Một số format phổ biến:
Code
Format
101
mm/dd/yyyy
103
dd/MM/yyyy
112
yyyymmdd
120
yyyy-mm-dd hh:mi:ss
121
yyyy-mm-dd hh:mi:ss.mmm
```

21.14, FORMAT() — Format ngày theo .NET
(Dễ dùng nhưng chậm)
```
SELECT FORMAT(GETDATE(), 'dd/MM/yyyy');
SELECT FORMAT(GETDATE(), 'dddd, MMMM dd yyyy');
```

21.15, SWITCHOFFSET()
Chuyển datetimeoffset sang timezone khác.

21.16. TODATETIMEOFFSET()
Thêm timezone vào datetime.

21.17, ISDATE()
Kiểm tra chuỗi có phải ngày hợp lệ không.
`SELECT ISDATE('2024-02-30');  -- 0 (không hợp lệ)`
