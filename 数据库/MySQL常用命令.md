#### 查看数据库大小



#### 查看表最后修改时间



#### 查看表信息

```sql
select * from information_schema.TABLES t where t.TABLE_SCHEMA = '数据库名' and t.TABLE_NAME = '表名';
```



#### 导出入.sql文件

**导出sql文件**

整库的表结构+数据

```shell
mysqldump -h$host -P$port -u$username -p$password $dbname > dbname.sql
```

某个表结构+数据

```shell
mysqldump -h$host -P$port -u$username -p$password $dbname $table > dbname.sql
```

仅表结构

```shell
mysqldump -h$host -P$port -u$username -p$password -d $dbname > dbname.sql
```

**导入sql文件**

```sql
mysql>source /home/admin/dbname.sql
```

或

```shell
mysql -h$host -P$port -u$username -p$password $dbname < dbname.sql
```

