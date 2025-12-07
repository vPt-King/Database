# Replace NULL in SQL server
1. ISNULL function

```
select E.Name as Employee , ISNULL(M.Name, ‘No manager’) as Manager 
from tblEmployee E
left join tblEmployee M
on E.ManagerID = M.EmployeeID
```

2. CASE Statement
```
select E.Name as Employee , 
case 
	when M.Name is NULL then ‘NO MANAGER’ else M.Name
end
as M.Manager
from tblEmployee E
left join tblEmployee M on E.ManagerID = M.EmployeeID
```

3. COALESCE function
COALESCE trong SQL Server là hàm trả về giá trị không NULL đầu tiên trong danh sách các biểu thức.
Hiểu đơn giản:
COALESCE(a, b, c, …) → trả về giá trị đầu tiên khác NULL từ trái sang phải.

```
select E.Name as Employee , COALESCE(M.Name, ‘No manager’) as Manager 
from tblEmployee E
left join tblEmployee M
on E.ManagerID = M.EmployeeID
```