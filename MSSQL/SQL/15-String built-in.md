
# String buil in funtion in sql server
## LEN() – Độ dài chuỗi (không tính khoảng trắng cuối)
`SELECT LEN('Hello');  -- 5`

## DATALENGTH() – Độ dài chuỗi tính theo byte
`SELECT DATALENGTH('Hello');  -- 5 bytes`


## LOWER() / UPPER()
```
SELECT LOWER('HELLO');  -- hello
SELECT UPPER('hello');  -- HELLO
```

## LEFT() / RIGHT()
```
SELECT LEFT('SQL Server', 3);   -- SQL
SELECT RIGHT('SQL Server', 6);  -- Server
```
## CHARINDEX() – Tìm vị trí
`SELECT CHARINDEX('er', 'Server');  -- 4`


## PATINDEX() – Tìm theo pattern (có wildcards)
`SELECT PATINDEX('%ver%', 'SQL Server'); -- 5`

## SUBSTRING() – Cắt chuỗi

`SELECT SUBSTRING('SQL Server', 5, 6);  -- Server`


## REPLACE() – Thay thế
`SELECT REPLACE('Hello World', 'World', 'SQL');  -- Hello SQL`


## LTRIM() / RTRIM() – Xóa khoảng trắng trái/phải
```
SELECT LTRIM('   hello');  -- 'hello'
SELECT RTRIM('hello   ');  -- 'hello'
```

10, CONCAT() – Nối chuỗi an toàn NULL**
`SELECT CONCAT('Hello ', NULL, 'SQL');  -- Hello SQL`


## CONCAT_WS() – Nối chuỗi có separator
`SELECT CONCAT_WS('-', 'A', 'B', 'C');  -- A-B-C`

## REPLICATE() – Lặp chuỗi
`SELECT REPLICATE('*', 5);  -- *****`

## STUFF() – Chèn chuỗi vào vị trí bất kỳ
`SELECT STUFF('Hello', 2, 3, 'i');  -- Hilo`


## ASCII() / UNICODE()
```
SELECT ASCII('A');       -- 65
SELECT UNICODE(N'Đ');    -- mã unicode
```
## CHAR() / NCHAR() – Trả về ký tự từ mã ASCII/Unicode
`SELECT CHAR(65);   -- 'A'`


## FORMAT() – Format số/ngày thành chuỗi
`SELECT FORMAT(GETDATE(), 'dd/MM/yyyy');`


## STRING_SPLIT() – Tách chuỗi thành nhiều dòng
`SELECT * FROM STRING_SPLIT('A,B,C', ',');`


## STRING_AGG() – Gộp nhiều dòng thành chuỗi
`SELECT STRING_AGG(Name, ',') FROM Users;`