## 序

2022年7月15日开始阅读本书，本书中覆盖浅显、复杂的知识点，阅读本书的目标是学习掌握自己对MySQL不了解的地方，并整理成笔记，而对于习以为常的知识点则忽略。本书共22章，455页，计划在1个月内时间读完，平均每天1章。

## 第4章 InnoDB记录存储结构

### 4.2 InnoDB页介绍

InnoDB将数据划分若干页，从磁盘读取到内存、从内存写入磁盘。InnoDB页就是的磁盘和内存交互的基本单位。

InnoDB页默认为16KB，数据库目录初始化时指定，后续不可修改。

### 4.3 InnoDB行格式

创建/修改表时可指定“行格式”，InnoDB行格式有COMPACT、REDUNDANT、DYNAMIC、COMPRESSED。

```sql
create table 表名 (列信息) row_format=行格式
alter table 表名 row_format=行格式
```

#### 4.3.2 COMPACT

