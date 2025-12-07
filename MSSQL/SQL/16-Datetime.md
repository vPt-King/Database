#  Datetime in sql
## GETDATE()
Trả về ngày giờ hiện tại (datetime).
`SELECT GETDATE();`


## SYSDATETIME()
Trả về ngày giờ hiện tại, độ chính xác cao hơn (datetime2).
`SELECT SYSDATETIME();`


## CURRENT_TIMESTAMP
Tương đương GETDATE(), chuẩn ANSI.
`SELECT CURRENT_TIMESTAMP;`


## GETUTCDATE() / SYSUTCDATETIME()
Ngày giờ UTC.
```
SELECT GETUTCDATE();
SELECT SYSUTCDATETIME();
```

## DATEADD() — Cộng/trừ thời gian
```
SELECT DATEADD(DAY, 7, GETDATE());  -- +7 ngày
SELECT DATEADD(MONTH, -1, GETDATE()); -- -1 tháng
```

## DATEDIFF() — Khoảng cách 2 ngày
`SELECT DATEDIFF(DAY, '2024-01-01', '2024-01-10');  -- 9`


## DATEDIFF_BIG()
Giống DATEDIFF nhưng dùng BigInt (hàng tỷ mili giây).

## DATEFROMPARTS()
Tạo ngày từ năm/tháng/ngày.
`SELECT DATEFROMPARTS(2024, 12, 20);`


## DATETIMEFROMPARTS()
Tạo datetime từ tất cả thành phần.

`SELECT DATETIMEFROMPARTS(2024, 12, 20, 10, 30, 0, 0);`


## EOMONTH() — Lấy ngày cuối tháng
```
SELECT EOMONTH(GETDATE());              -- Cuối tháng này
SELECT EOMONTH(GETDATE(), 1);           -- Cuối tháng sau
```

## DATEPART() — Lấy từng phần của ngày
```
SELECT DATEPART(YEAR, GETDATE());   -- Năm
SELECT DATEPART(MONTH, GETDATE());  -- Tháng
SELECT DATEPART(WEEKDAY, GETDATE());-- Thứ trong tuần
```

## DATENAME() — Lấy tên dạng chữ
`SELECT DATENAME(WEEKDAY, GETDATE());  -- Monday, Tuesday...`

## CONVERT() — Định dạng ngày (format)
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

## FORMAT() — Format ngày theo .NET
(Dễ dùng nhưng chậm)
```
SELECT FORMAT(GETDATE(), 'dd/MM/yyyy');
SELECT FORMAT(GETDATE(), 'dddd, MMMM dd yyyy');
```

## SWITCHOFFSET()
Chuyển datetimeoffset sang timezone khác.

## TODATETIMEOFFSET()
Thêm timezone vào datetime.

## ISDATE()
Kiểm tra chuỗi có phải ngày hợp lệ không.
`SELECT ISDATE('2024-02-30');  -- 0 (không hợp lệ)`
