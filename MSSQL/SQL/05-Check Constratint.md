# Check constraint 
Check constraint is responsible for checking the value which is updated to the column

If the value updated to the row does not satisfied the boolean expression, then it will not allow to insert to the column

1. Add check constraint 
```
Alter table {table_name} add constraint {constraint_name} check {boolean_expression}

alter table tblPerson add constraint ck_person_age check (AGE > 0 and AGE < 150 )
```

2. Drop check constraint 
```
Alter table {table_name} drop constraint {constraint_name}
```