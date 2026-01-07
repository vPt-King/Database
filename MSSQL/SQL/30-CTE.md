# Common table expression
là một bảng tạm thời có tên, được định nghĩa trong phạm vi một câu SQL, giúp câu query dễ đọc, dễ hiểu và dễ bảo trì hơn.

# 1. Cú pháp cơ bản của CTE
```
WITH cte_name AS (
    SELECT column1, column2
    FROM table_name
    WHERE condition
)
SELECT *
FROM cte_name;
```
CTE chỉ tồn tại trong câu lệnh SQL đó, không lưu trong database.

# 2. CTE dùng để làm gì?
✅ Mục đích chính
Thay thế subquery phức tạp
Viết SQL rõ ràng, logic hơn
Dùng cho đệ quy (recursive query) – rất quan trọng
Dễ debug và mở rộng

# 3. Ví dụ dễ hiểu
SELECT *
FROM (
    SELECT employee_id, department_id
    FROM employees
    WHERE salary > 2000
) t
WHERE department_id = 10;

Dùng CTE (dễ đọc hơn)

WITH high_salary_emp AS (
    SELECT employee_id, department_id
    FROM employees
    WHERE salary > 2000
)
SELECT *
FROM high_salary_emp
WHERE department_id = 10;

# 4. CTE đệ quy (Recursive CTE) – cực kỳ quan trọng

Dùng để xử lý:

Cây thư mục

Cấu trúc cha–con

Organizational chart

Comment lồng nhau (giống hệ thống comment Facebook)

```
WITH emp_tree AS (
    -- Anchor (gốc)
    SELECT employee_id, manager_id, employee_name
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive
    SELECT e.employee_id, e.manager_id, e.employee_name
    FROM employees e
    JOIN emp_tree et ON e.manager_id = et.employee_id
)
SELECT * FROM emp_tree;
```


# 1️⃣ CTE có UPDATE được không?

👉 CÓ, nếu CTE tham chiếu trực tiếp đến 1 bảng duy nhất
👉 KHÔNG, nếu CTE:

Join nhiều bảng

Có GROUP BY, DISTINCT, UNION

Có aggregate (SUM, COUNT, …)

📌 Nguyên tắc:

CTE update được khi nó “updatable”

# 2️⃣ Ví dụ UPDATE đơn giản với CTE (chuẩn nhất)
Bảng:
Employees
---------
Id | Name | Salary | Dept

Tăng lương 10% cho phòng IT
```
WITH cte_IT AS (
    SELECT *
    FROM Employees
    WHERE Dept = 'IT'
)
UPDATE cte_IT
SET Salary = Salary * 1.1;
```
✔️ Hoạt động
✔️ Update trực tiếp bảng Employees

# 3️⃣ UPDATE CTE có JOIN (trường hợp hay dùng)
Bảng:
```
Orders(OrderId, CustomerId, Amount)
Customers(CustomerId, Type)
```
Tăng 5% Amount cho khách VIP
```
WITH cte AS (
    SELECT o.*
    FROM Orders o
    JOIN Customers c ON o.CustomerId = c.CustomerId
    WHERE c.Type = 'VIP'
)
UPDATE cte
SET Amount = Amount * 1.05;
```
✔️ OK vì:
CTE chỉ update Orders
Customers chỉ dùng để lọc
# 4️⃣ ❌ Trường hợp UPDATE CTE bị lỗi
## ❌ CTE có GROUP BY
```
WITH cte AS (
    SELECT Dept, SUM(Salary) AS TotalSalary
    FROM Employees
    GROUP BY Dept
)
UPDATE cte
SET TotalSalary = TotalSalary * 1.1;
```
❌ Lỗi: không update được derived data
## ❌ CTE có DISTINCT
```
WITH cte AS (
    SELECT DISTINCT Dept
    FROM Employees
)
UPDATE cte
SET Dept = 'HR';
```
❌ Không xác định row gốc

# Ví dụ nâng cao: UPDATE với ROW_NUMBER()
## Xóa lương trùng → giữ record mới nhất

```
WITH cte AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY Name ORDER BY Id DESC) AS rn
    FROM Employees
)
UPDATE cte
SET Salary = 0
WHERE rn > 1;
```
✔️ CTE cho phép window function
✔️ Bảng gốc vẫn update được