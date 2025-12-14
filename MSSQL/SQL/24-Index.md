# Indexes
```
- Indexes are used by queries to find data from tables quickly using B-Tree. Indexes are created on tables and views. 
- SELECT Faster 
- WHERE , JOIN, ORDER BY, GROUP more efficient
- But INSERT / UPDATE / DELETE will slow
```
To check index in one table using:
`sp_Helpindex [table name]`

To drop table:
`drop index [index name]`

## Bản chất index
SQL Server 
Đơn vị lưu trữ nhỏ nhất: PAGE (8 KB)
```
File (.mdf)
 ├─ Page 1 (8KB)
 ├─ Page 2 (8KB)
 ├─ Page 3 (8KB)
```
👉 Database KHÔNG lưu từng dòng, mà lưu nhiều dòng trong 1 page.
Một bảng mới (CHƯA CÓ INDEX)
➡ Bảng lúc này gọi là HEAP
```
Heap Table
 ├─ Page 1: row A, row B
 ├─ Page 2: row C, row D
 ├─ Page 3: row E
```
📌 Thứ tự:
Theo thứ tự insert
Không có sắp xếp
Khi bạn SELECT mà KHÔNG có index
`SELECT * FROM Orders WHERE OrderId = 1000;`
SQL Server không biết dòng nằm ở đâu => Đọc Page 1 → Page 2 → Page 3 → ...
=> Table Scan

Khi tạo NONCLUSTERED INDEX
`CREATE INDEX IX_Orders_OrderId ON Orders(OrderId);`
Database tạo thêm 1 cấu trúc mới là B tree
```
Nonclustered Index (B-Tree)
 ├─ Root
 ├─ Intermediate
 └─ Leaf
```
Leaf page chứa:
```
OrderId | Row Locator
```
Row Locator là:
Heap → RID (File:Page:Slot)
Có clustered → Clustered Key
📌 Index không chứa full row
SELECT khi có NONCLUSTERED INDEX:
`WHERE OrderId = 1000`
SQL Server làm:
- Dùng B-Tree → Index Seek → tìm leaf
- Lấy Row Locator
- Nhảy tới đúng page của bảng
- Đọc row

➡ Ít page hơn → nhanh hơn

📌 Nhưng vẫn 2 bước

Khi bạn tạo CLUSTERED INDEX
```
CREATE CLUSTERED INDEX IX_Orders_OrderId
ON Orders(OrderId);
```
🔥 Đây là bước “đổi đời”

👉 Database KHÔNG còn heap

➡ Dữ liệu bảng được tổ chức lại thành B-Tree
```
Clustered Index
 ├─ Root
 ├─ Intermediate
 └─ Leaf = DATA PAGE
```
Leaf chính là dữ liệu thật
SELECT khi có CLUSTERED INDEX
`WHERE OrderId = 1000`
SQL Server làm:
```
- Root → Intermediate → Leaf
- Đọc row
```

➡ KHÔNG lookup thêm
➡ Ít I/O hơn nonclustered

#Tóm tắt
🔹 Database lưu theo PAGE, không theo row
🔹 Heap = không chỉ đường → scan
🔹 Index = B-Tree chỉ đường
🔹 Nonclustered → chỉ đường + lookup
🔹 Clustered → chỉ đường = dữ liệu
🔹 Khác nhau ở leaf page chứa gì

