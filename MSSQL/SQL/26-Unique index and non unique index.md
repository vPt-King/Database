# Unique index
Unique Index là một loại index dùng để đảm bảo dữ liệu trong cột (hoặc tập cột) là duy nhất, tức là không có 2 dòng nào có cùng giá trị trên các column đó.

👉 Ngoài việc tăng tốc truy vấn, Unique Index còn đảm bảo ràng buộc toàn vẹn dữ liệu.
```
- PRIMARY KEY CONSTRAINT create a unique clustered index
- UNIQUE CONSTRAINT create a unique nonclustered index
- Unique Constraint and UNIQUE index cannot create on the tables if duplicate values is existed in key column
```

## Unique Index hoạt động như thế nào?
SQL Server không cho phép insert / update nếu giá trị mới trùng với giá trị đã tồn tại trong index.
Dữ liệu trong index được tổ chức dạng B-Tree (giống Nonclustered / Clustered Index).
Khi insert dữ liệu, SQL Server kiểm tra index trước → nếu trùng → báo lỗi.

## Cú pháp tạo uniue index
Unique Nonclustered Index:
```
CREATE UNIQUE INDEX IX_User_Email
ON dbo.Users(Email);
```

Unique Clustered Index
```
CREATE UNIQUE CLUSTERED INDEX IX_User_ID
ON dbo.Users(UserID);
```
Dữ liệu bảng được sắp xếp vật lý theo UserID
Mỗi UserID là duy nhất
📌 Mỗi bảng chỉ có 1 clustered index

## Filtered Unique Index (rất hay dùng)
Chỉ áp dụng uniqueness cho một phần dữ liệu
```
CREATE UNIQUE INDEX IX_User_Email_NotNull
ON dbo.Users(Email)
WHERE Email IS NOT NULL;
```
Cho phép:
```
Email = NULL
Email = NULL
```
Không cho:
```
test@gmail.com
test@gmail.com
```
📌 Cực kỳ phổ biến khi:
Email / Phone không bắt buộc
Nhưng nếu có thì phải unique

