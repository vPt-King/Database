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
Row Locator là: địa chỉ để DB tìm tới đúng dòng dữ liệu , địa chỉ vật lý trên disk, Chỉ xuất hiện trong NONCLUSTERED INDEX
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

# Tóm tắt
```
- Database lưu theo PAGE, không theo row
- Heap = không chỉ đường → scan
- Index = B-Tree chỉ đường
- Nonclustered → chỉ đường + lookup
- Clustered → chỉ đường = dữ liệu
- Khác nhau ở leaf page chứa gì
```
# Index gắn với chọn Column
## Vì sao index phải gắn với column?
👉 Vì database tìm dữ liệu dựa trên GIÁ TRỊ, không dựa trên dòng.
Index không biết:
bạn muốn tìm dòng thứ mấy
bạn muốn tìm “bản ghi nào đó”
Index chỉ biết:
“Tôi có danh sách các giá trị của cột X, được sắp xếp, và mỗi giá trị chỉ ra dòng dữ liệu tương ứng”
📌 Nên bắt buộc phải chọn column khi tạo index.

## Index thực sự lưu cái gì (nhắc lại rất quan trọng)
`CREATE INDEX IX_Users_Email ON Users(Email);`

Index sẽ lưu kiểu:
```
Email                → RowLocator
----------------------------------
a@gmail.com          → (ID = 1001)
b@gmail.com          → (ID = 1050)
c@gmail.com          → (ID = 2000)
```
➡ Không có Email → index không có gì để tra

## Khi nào WHERE “kích hoạt” index?
👉 KHÔNG phải cứ có WHERE là dùng index
Index chỉ được dùng khi:
✅ Điều kiện WHERE liên quan trực tiếp đến cột được index
```
WHERE Email = 'a@gmail.com'     -- dùng index
WHERE Email LIKE 'a%'           -- dùng index
```
❌ KHÔNG dùng index nếu:
```
WHERE UPPER(Email) = 'A@GMAIL.COM'
WHERE YEAR(CreatedDate) = 2024
WHERE Salary + 1000 > 5000
```
➡ Vì DB không dùng trực tiếp giá trị gốc trong index

# Các loại index
```
Index	Cấu trúc	Leaf chứa	Dùng cho
Clustered	B-Tree	Data	OLTP, range
Nonclustered	B-Tree	Key + locator	OLTP
Composite	B-Tree	Tùy loại	Multi-column
Covering	B-Tree	Key + data	Tránh lookup
Unique	B-Tree	Key	Ràng buộc
Filtered	B-Tree	Partial data	Selective
Columnstore	Column	Column segments	OLAP
```