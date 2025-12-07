# Cascading referential integrity constraint
This is allows to define the actions of SQL server should take when a row which is existed foreign key is updated or deleted
By default an error will occur if this happen

Option when setting up Cascading referential integrity constraint:
1. No action
This is the default behaviour. No action specifies that if an attempt is made to delete or update row with a  key referenced by foreign keys in existing rows in other tables, an error is raised and the delete or update is rollback

2. Cascade 
Specifies that if an attempt is made to delete or update a row with a key referenced by foreign key in existing row in other tables , all rows containing those foreign keys are also deleted or updated.
3. Set Null
Set null if a key is deleted or updated

4. Set Default
Default values is set when a key is deleted or updated

5. Command
```
ALTER TABLE <ten_bang_con>
ADD CONSTRAINT <ten_constraint>
FOREIGN KEY (<cot_fk>)
REFERENCES <ten_bang_cha>(<cot_pk>)
ON DELETE { NO ACTION | CASCADE | SET NULL | SET DEFAULT }
ON UPDATE { NO ACTION | CASCADE | SET NULL | SET DEFAULT };
```

Remember: FK ở bảng con được set về giá trị default của cột đó