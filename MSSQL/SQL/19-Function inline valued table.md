# Inline Table Valued function
Inline table valued function return a table
+ We specify TABLE as a return type
+ The function body does not enclosed between BEGIN and END block
+ The SELECT statement determine the structure of returned table
+ Only have one SELECT statement
+ Can using join in Function
+ We can use the returned table to join with another table

## Structure of function
```
CREATE FUNCTION fn_GetOrdersByCustomer
(
    @CustomerID INT
)
RETURNS TABLE
AS
RETURN
(
    SELECT OrderID, OrderDate, TotalAmount
    FROM Orders
    WHERE CustomerID = @CustomerID
);
```

Example:
```
CREATE FUNCTION fn_GetPersonByGender
(
	@Gender VARCHAR(10)
)
RETURNS TABLE
AS
RETURN 
(
	SELECT p.Email , p.Name, g.Gender 
	FROM dbo.tblPerson p
	INNER JOIN dbo.tblGender g 
	ON p.GenderID = g.ID
    WHERE g.Gender = @Gender
);
```

