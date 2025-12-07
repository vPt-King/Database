# Stored Procedure
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
## Stored Procedure đơn giản
```
CREATE PROCEDURE GetAllUsers
AS
BEGIN
    SELECT * FROM Users;
END;

Gọi procedure
EXEC GetAllUsers;
```


## Stored Procedure có tham số
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

## Stored Procedure với INSERT/UPDATE/DELETE

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

## Stored Procedure trả về giá trị (return)
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