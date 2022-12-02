大量转移数据





pg从不同库导表及数据

`pg_dump -C -h ip -U  username dbname | psql -h ip -U username dbname`





创建用户

`psql -c "create user username with password 'password';"`



创建数据库并指定用户

`create dbname -O username`



映射导数

`psql -h ip -U username -c "\copy (select column1,column2,3 column3 from table_a where column1>10 ) to "`