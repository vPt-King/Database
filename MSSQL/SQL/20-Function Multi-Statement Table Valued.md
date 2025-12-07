# Multi statement table valued function
+ The function body enclosed between begin and end block
+ Must declare the structure of returned table
+ Can using multiple query in the body function

## Structure of function
```
CREATE FUNCTION fn_GetLargeOrders
(
    @MinAmount DECIMAL(18,2)
)
RETURNS @Result TABLE
(
    OrderID INT,
    CustomerID INT,
    Amount DECIMAL(18,2)
)
AS
BEGIN
    INSERT INTO @Result
    SELECT OrderID, CustomerID, Amount
    FROM Orders
    WHERE Amount >= @MinAmount;

    -- Thậm chí có thể thêm logic khác
    UPDATE @Result
    SET Amount = Amount * 1.1; -- ví dụ tính thuế

    RETURN;
END;

```

## When to use
Khi nào cần dùng MSTVF?
+ Dùng khi logic không thể viết trong 1 SELECT, ví dụ:
+ Đổ dữ liệu vào nhiều lần (INSERT nhiều lần)
+ Có nhiều bước xử lý
+ Có IF/ELSE, WHILE
+ Cần biến trung gian (DECLARE …)

## When not to use
Multi-statement TVF có hiệu năng rất kém.

Lý do:
+ SQL Server không tối ưu được vì kết quả trong MSTVF là bảng tạm (@table variable)
+ Cardinality luôn bị ước lượng = 1 row → dẫn tới JOIN bị sai kế hoạch
+ Có thể gây tràn tempdb, chạy rất chậm khi data lớn
Đây là lý do người ta luôn khuyên:
Nếu viết được inline TVF thì nên dùng inline TVF, SQL server coi Inline function như là view còn MSTVF giống như là procedure
