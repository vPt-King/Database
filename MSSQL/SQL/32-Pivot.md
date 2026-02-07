\# Pivot là gì

PIVOT dùng để xoay dữ liệu từ dạng hàng (rows) sang dạng cột (columns).

👉 Nói nôm na:

Biến giá trị trong một cột thành nhiều cột mới.



\# Ví dụ



Bảng chưa pivot 

```

Bảng Sales

Year	Quarter	Amount

2024	Q1	100

2024	Q2	200

2024	Q3	150

```



👉 Muốn ra dạng:

```

Year	Q1	Q2	Q3

2024	100	200	150

```



Dùng pivot:

```

Select \* 

from (

&nbsp;	Select year, Quarter, Amount

&nbsp;	From sales

) AS src 					# (Prepare raw data)

PIVOT (

&nbsp;	SUM(Amount) 				# (Required aggregate)

&nbsp;	For Quarter in (\[Q1], \[Q2], \[Q3]	# Quarter : Rotated column, 	

) as p;						# New column \[Q1], \[Q2], \[Q3] 

```



\# Note

Always have aggregate (SUM, COUNT, MAX,...)

The columns in IN(...) must be known in adavance

Not suitable if the data is dynamic and has many values







