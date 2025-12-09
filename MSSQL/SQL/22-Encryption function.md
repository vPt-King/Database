# Use WITH ENCRYPTION
📌 Mục đích
Ẩn (obfuscate) phần code định nghĩa của Stored Procedure, Function, hoặc View để người khác không xem được nội dung khi dùng:
`sp_helptext object_name`
hoặc khi mở trong SSMS.
Chỉ ẩn code chứ không mã hóa thật sự

SQL Server chỉ làm “obfuscate”, không phải encryption mạnh.
Vẫn có thể bị giải mã bằng tool chuyên dụng → đừng dùng để bảo mật tuyệt đối.

Dùng khi nào
Khi bạn không muốn Dev khác xem code logic của bạn
Khi đóng gói code SQL để giao cho khách mà không muốn họ copy.

```
CREATE FUNCTION dbo.fn_Test()
RETURNS INT
WITH ENCRYPTION
AS
BEGIN
    RETURN 100;
END;
```

# WITH SCHEMABINDING trong SQL Server
📌 Mục đích

Buộc SQL Server khóa schema của các bảng mà View hoặc Function đang sử dụng → không cho đổi tên hoặc xóa bảng/cột.

Nghĩa là:
Nếu bạn tạo view/function phụ thuộc bảng A, thì bảng A không được phép:

ALTER TABLE ... DROP COLUMN

DROP TABLE ...

RENAME TABLE ...

ALTER COLUMN kiểu dữ liệu thay đổi
Nếu muốn thay đổi, phải DROP hoặc ALTER view/function để bỏ SCHEMABINDING trước.

Yêu cầu khi dùng SCHEMABINDING
Phải dùng schema prefix: dbo.TableName
Không được dùng SELECT *
Tất cả bảng tham chiếu phải nằm trong cùng database

Dùng khi nào
Khi bạn muốn view hoặc function ổn định, tránh bị hỏng do người khác thay đổi cấu trúc bảng.
Khi tạo Indexed View bắt buộc phải dùng SCHEMABINDING.

```
CREATE VIEW dbo.vw_ProductSales
WITH SCHEMABINDING
AS
SELECT 
    p.ProductID,
    p.ProductName,
    p.Price
FROM dbo.Products p;
GO
```

# So sánh nhanh
```
Option	Tác dụng	Hạn chế	Dùng cho
WITH ENCRYPTION	Ẩn code, không cho người khác xem	Có thể bị giải mã bằng tool	SP, Function, View
WITH SCHEMABINDING	Khóa schema, không cho thay đổi bảng/cột liên quan	Bắt buộc dùng schema prefix + không dùng *	View, Function
```