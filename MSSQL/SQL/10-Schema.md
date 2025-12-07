# Schema
Schema trong SQL Server là một “nhóm” (namespace) dùng để chứa các đối tượng trong database, như:
Table (bảng)
View
Stored Procedure
Function
Trigger
Synonym
Type


Nó giống như folder hoặc namespace để tổ chức các đối tượng trong cơ sở dữ liệu.
1. Định nghĩa chính xác
Schema = một container logic để quản lý và phân quyền các object trong database.
Ví dụ:
dbo.Customers
sales.Orders
hr.Employees
report.DailyRevenue
Schema đứng trước tên bảng, phân biệt các đối tượng có cùng tên nhưng khác schema.

2. Tại sao lại cần schema
Database lớn có thể có hàng trăm bảng → chia theo schema sẽ dễ quản lý:
dbo → bảng hệ thống chính
sales → nghiệp vụ bán hàng
hr → nhân sự
audit → bảng log
report → các view báo cáo

3. Phân quyền
Bạn có thể cấp quyền theo schema, không cần cấp từng bảng:
GRANT SELECT ON SCHEMA::sales TO userA;

User A chỉ xem được bảng trong schema sales.

4. Schema mặc định
Schema mặc định là dbo.
Nghĩa là khi bạn tạo bảng:
CREATE TABLE Customers (...);

Nó thật ra là:
CREATE TABLE dbo.Customers (...);
5. Tạo schema mới
CREATE SCHEMA sales;
GO

CREATE TABLE sales.Orders (
    OrderID INT,
    Amount DECIMAL(10,2)
);


6. Chuyển 1 table sang 1 schema khác
`ALTER SCHEMA sales TRANSFER dbo.Customers;`

Schema là 1 namespace trong database dùng để:
Tổ chức các bảng và đối tượng
Phân quyền dễ dàng
Tránh trùng tên object
Một database có thể có nhiều schema
Schema quan trọng như thư mục trong filesystem vậy.

7. Kiểm tra schema trong database
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
8. Mối quan hệ User và Schema
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