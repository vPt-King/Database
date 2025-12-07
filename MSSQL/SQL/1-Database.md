# Create, alter, drop database
1. Tạo database: 
`CREATE DATABASE sample1`;

Khi tạo 1 database mới hay là system database đều sẽ tạo ra 2 file : .mdf và .ldf
.mdf file - actual data
.ldf file - transaction log data ( used to recover data )

2. Đổi tên database:
`alter database sample1 modify name=sample3;`

hoặc sử dụng procedure: 
`sp_renameDB 'sample3','sample4';`

3. Delete database
Chỉ xóa db khi không có connection nào vào database đó
sau khi delete database thì nó sẽ xóa các file .mdf và .ldf tương ứng với db đó

`drop database sample4;`
Nếu có connection thì ta sẽ set về chế độ single user mode và có option Rollback immediate để rollback các incomplete transaction
`alter database ‘database_name’ set SINGLE_USER with rollback immediate;`

System database không thể drop