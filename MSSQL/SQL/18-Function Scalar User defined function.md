There are 3 types of User Defined function
+ Scalar function
+ Inline table-valued function
+ multi-statement table-valued function

# Scalar function
1. Definition
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

To invoke function 
`Select {schema}.{Name_Function}`

To alter a function we use ALTER FUNCTION {FunctionName}
To delete a function we use DELETE FUNCTION {FunctionName}

2. Used in Select clause
`Select {FuntionName[parameter]} from A`

3. Used in Where clause
`Select X from A where {FuntionName[parameter]} condition`
