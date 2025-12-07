There are 3 types of User Defined function
+ Scalar function
+ Inline table-valued function
+ multi-statement table-valued function

# Scalar function
Scalar function may or may not have parameter but always return a value.
The datatype of value not text,  ntext , image, video, cursor and timestamp
```
--To create a function, we use the following syntax:
CREATE FUNCTION Function_Name (@Parameter1 DataType, @Parameter2 DataType,...@ParameterN DataType)
RETURNS Return_Datatype
AS
BEGIN
    --Function Body
    Return Return_Datatype
END
```