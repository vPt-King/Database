1. ABS function
Return absolute value of a number

`Select ABS(-100) --return 100`

2. CELLING/FLOOR function
Celling returns the smallest integer value greater than or equal to the parameter
Floor returns the largest integer value less than or equal to the parameter

```
Select CEILING(15.2) -- 16
Select CEILING(-15.2) -- -15

Select FLOOR(15.2) -- 15
Select FLOOR(-15.2) -- -16
```

3. Power function
Return the power value of the specificed expression to the specified power
`Select POWER(2,3) -- return 8`

4. SQUARE function
return the square of given number
`Select SQUARE(9) -- return 81`

5. Square root
return the square root of given number
`Select SQRT(81) -- return 9`

6. Rand() function
Return a random float value between 0 and 1
`Select RAND()`

To return a random number from N->M:
`SELECT FLOOR(RAND() * (M - N + 1)) + N;`

RAND() wil create a number for whole select statement, to create a new value for each row you must using NewID()
`SELECT ABS(CHECKSUM(NEWID())) % 100 AS RandomNumber FROM Users;`

7. ROUND(numeric_expression, length [, function])
Rounds the given numeric expression based on the given length, this function takes 3 parameter
Tham số

+ numeric_expression: số bạn muốn làm tròn.

+ length:
số chữ số thập phân muốn làm tròn.
length > 0 → làm tròn bên phải dấu thập phân.
length = 0 → làm tròn đến số nguyên.
length < 0 → làm tròn phía bên trái dấu thập phân (hàng chục, hàng trăm…)

+ function (tùy chọn):
0 hoặc bỏ trống → làm tròn bình thường (round half up).
1 → cắt bỏ (truncate), không làm tròn.

Ví dụ
Làm tròn đến 2 chữ số thập phân
SELECT ROUND(12.3456, 2);  
Kết quả: 12.35

Làm tròn đến 0 chữ số (lấy số nguyên)
SELECT ROUND(12.5, 0);
Kết quả: 13

Làm tròn phía bên trái dấu thập phân
SELECT ROUND(1234.56, -2);
Kết quả: 1200

Dùng function = 1 (cắt bỏ, không làm tròn)
SELECT ROUND(12.999, 2, 1);
Kết quả: 12.99 (không làm tròn lên)

Ví dụ làm tròn giá trị tiền
SELECT ROUND(15000.678, 0);     -- 15001
SELECT ROUND(15000.678, 2);     -- 15000.68

Kết hợp ROUND và tính toán
SELECT ROUND(Salary * 1.055, 2) AS NewSalary
FROM Employees;