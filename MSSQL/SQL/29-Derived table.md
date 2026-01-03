# Derived table
```
Trong SQL Server, derived table là một bảng tạm thời được tạo ra từ một câu lệnh SELECT con (subquery) và chỉ tồn tại trong phạm vi của câu query đó.

Nói đơn giản:
👉 Derived table = subquery trong mệnh đề FROM.
```

## Đặc điểm của derived table
```
Được viết trong FROM

Bắt buộc phải có alias

Không lưu trên database

Chỉ dùng được trong câu query hiện tại

Giống bảng thật để JOIN, WHERE, GROUP BY…
```

## Cú pháp cơ bản
```
SELECT *
FROM (
    SELECT id, name, salary
    FROM Employees
    WHERE salary > 1000
) AS HighSalaryEmployees;
```
Phần trong ngoặc (SELECT ...) chính là derived table
HighSalaryEmployees là alias (bắt buộc)

## Ví dụ
Ví dụ 1: Lọc dữ liệu trước rồi mới JOIN
```
SELECT d.department_name, e.name, e.salary
FROM (
    SELECT *
    FROM Employees
    WHERE salary > 2000
) e
JOIN Departments d ON e.department_id = d.id;
```
Ở đây:
Derived table e chỉ chứa nhân viên lương > 2000
Sau đó mới JOIN với bảng Departments

Ví dụ 2: GROUP BY trong derived table
```
SELECT department_id, avg_salary
FROM (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM Employees
    GROUP BY department_id
) t
WHERE avg_salary > 3000;
```

## Khi nào nên dùng derived table
```
✅ Khi:

Query ngắn, logic đơn giản

Cần lọc / group trước rồi xử lý tiếp

Không cần tái sử dụng

❌ Không nên dùng khi:

Query quá dài → khó đọc

Cần dùng lại nhiều lần → nên dùng CTE hoặc temp table
```