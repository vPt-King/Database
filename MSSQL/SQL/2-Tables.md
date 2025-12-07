
# Creating and working with tables
1. Constraint là gì
Constraint trong Database là ràng buộc để đảm bảo dữ liệu trong bảng luôn đúng, hợp lệ, nhất quán.
Nó giống như “luật” để ngăn người dùng nhập dữ liệu sai hoặc làm hỏng dữ liệu.
Popular Constraint:
```
primary key
foreign key
unique
not null
check
default
```
2. Primary key
Xác định duy nhất mỗi bản ghi trong bảng
Không được trùng
Không được NULL

3. FOREIGN KEY

4. Create table
```
create table tblGender(
	ID int NOT NULL primary key,
	Gender nvarchar(50) NOT NULL
);
```
5. alter thêm foreign key vào exist table
```
alter table <ForeignKeyTable> add constraint FK_ForeignKeyTable_PrimaryKeyTable
Foreign KEY (ForeignKeyColumn) references PrimaryKeyTable (PrimaryKeyColumn)
```
```
alter table dbo.tblPerson add constraint FK_tblPerson_tblGender
FOREIGN KEY (GenderID) references dbo.tblGender(ID);
```