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

#### 创建表结构

```sql
CREATE TABLE `info` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) DEFAULT NULL,
  `ctime` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `ignore` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) DEFAULT NULL,
  `ctime` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

#### 插入数据

```sql
INSERT INTO `ignore` (`name`)
VALUES
	('chenxue'),
	('chenxue2');

INSERT INTO `info` (`name`)
VALUES
	('chenxue'),
	('chenxue2');
```

