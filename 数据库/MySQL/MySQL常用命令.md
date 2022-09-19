

### 元数据

------

#### 查看数据库大小



#### 查看表最后修改时间



#### 查看表信息

```sql
select * from information_schema.TABLES t where t.TABLE_SCHEMA = '数据库名' and t.TABLE_NAME = '表名';
select *, truncate(data_length/1024/1024, 2) as data_length_MB, truncate(index_length/1024/1024, 2) as index_length_MB, truncate(data_free/1024/1024, 2) as data_free_MB  from information_schema.TABLES t where t.TABLE_SCHEMA = 'chenxue_db' and t.TABLE_NAME = 'new_node_entity'\G
```

#### 查看binlog

查看binlog内容（第一个binlog文件）

```sql
show binlog events;
```

查看binlog内容（指定binlog文件）

```sql
show binlog events in 'mysql-bin.000002';
```

查看当前写入的binlog文件

```sql
show master status;
```

查看binlog文件列表

```sql
show binary logs;
```



### DDL

------

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

```sql
ALTER TABLE node_entity DROP PRIMARY KEY;
alter table new_node_entity add column `new_id` bigint(20) NOT NULL PRIMARY KEY AUTO_INCREMENT COMMENT '测试主键ID';
```



### DML

------

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

#### 造数据

```sql
insert into node_entity select * from node_entity;
```

### DQL

------

>  待补充：数据查询语言

### DCL

------

> 待补充：用户，权限，事务。



### 其他

------

#### 导.sql文件

##### 导出sql文件

- 整库的表结构+数据

```shell
mysqldump -h$host -P$port -u$username -p$password $dbname > dbname.sql
```

- 某个表结构+数据

```shell
mysqldump -h$host -P$port -u$username -p$password $dbname $table > dbname.sql
```

- 仅表结构

```shell
mysqldump -h$host -P$port -u$username -p$password -d $dbname > dbname.sql
```

##### 导入sql文件

```sql
mysql>source /home/admin/dbname.sql
```

或

```shell
mysql -h$host -P$port -u$username -p$password $dbname < dbname.sql
```

### 
