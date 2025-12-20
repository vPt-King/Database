# Group by
GROUP BY trong SQL Server dùng để gom nhóm các dòng có giá trị giống nhau trong một hoặc nhiều cột, sau đó bạn có thể dùng hàm tổng hợp (aggregate functions) để tính toán trên từng nhóm.
Gom nhóm dữ liệu theo một hoặc nhiều cột

Kết hợp với hàm tổng hợp để tính:
COUNT() – đếm số dòng
SUM() – tính tổng
AVG() – tính trung bình
MAX() – giá trị lớn nhất
MIN() – giá trị nhỏ nhất
```
SELECT column1, column2, aggregate_function(column3)
FROM table_name
GROUP BY column1, column2;
```
Có thể kết hợp sử dụng where và having
where thì đứng trước group by là lọc trước khi gộp
having thì lọc sau khi gộp và viết sau group by