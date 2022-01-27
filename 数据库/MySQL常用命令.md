查看数据库大小



查看表最后修改时间



查看表信息

```sql
select * from information_schema.TABLES t where t.TABLE_SCHEMA = '数据库名' and t.TABLE_NAME = '表名';
```

