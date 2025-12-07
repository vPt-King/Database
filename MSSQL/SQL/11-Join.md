# Join
1. Inner join
Chỉ trả về phần giao nhau.
Ví dụ:
SELECT *
FROM A
INNER JOIN B ON A.id = B.a_id;

2. Left join
Lấy tất cả dữ liệu của bảng bên trái, và những dòng khớp ở bảng phải
 → Nếu bảng phải không có thì trả NULL.
Ví dụ:
SELECT *
FROM A
LEFT JOIN B ON A.id = B.a_id;

3. Right join
Ngược lại với LEFT JOIN.
 Lấy tất cả dữ liệu của bảng bên phải, và dữ liệu khớp ở bảng trái.
Ví dụ:
SELECT *
FROM A
RIGHT JOIN B ON A.id = B.a_id;

4. Full join
Lấy tất cả dữ liệu của cả 2 bảng,
 Dòng nào không khớp thì trả NULL ở phía còn lại.
Ví dụ:
SELECT *
FROM A
FULL OUTER JOIN B ON A.id = B.a_id;

5. Cross join
Nhân Cartesian – mỗi dòng của bảng A kết hợp với mỗi dòng của bảng B.
Ví dụ:
 A có 3 dòng, B có 4 dòng → kết quả 12 dòng.
SELECT *
FROM A
CROSS JOIN B;

6. Self join
JOIN chính nó (thường để xử lý dữ liệu dạng cây – parent/child).
Ví dụ (lấy nhân viên + quản lý của họ):
SELECT e.Name AS Employee, m.Name AS Manager
FROM Employee e
LEFT JOIN Employee m ON e.ManagerId = m.Id;

# Advance join
1. Retrive not maching left join
LEFT JOIN + lọc những dòng không match bằng cách check NULL của cột trong điều kiện on ở bảng bên phải.
Công thức chung:
```
SELECT *
FROM A
LEFT JOIN B ON A.key = B.key
WHERE B.key IS NULL;
```
2. Retrive not maching right join
RIGHT JOIN + lọc những dòng không match bằng cách check NULL của cột trong điều kiện on ở bảng bên trái.
Công thức chung:
```
SELECT *
FROM A
RIGHT JOIN B ON A.key = B.key
WHERE A.key IS NULL;
```

3. Retrive not match inner join

Công thức chung:
```
SELECT *
FROM A
RIGHT JOIN B ON A.key = B.key
WHERE A.key IS NULL or B.key is NULL;
```