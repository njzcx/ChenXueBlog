### 容量限制

- 单表的最大尺寸通常受限于操作系统的文件大小限制，而不是受MySQL内部机制的限制。由于存在部分元数据的开销，MySQL单表的最大尺寸为略小于2 TB。如果你使用自增 ID，那最大就可以存储 2^32 或 2^64 条记录了，这是按自增 ID 的数据类型 int 或 bigint 来计算的。

  - 建议控制单表数据量在如下范围以保证良好的性能：表中记录数在2000万条以内。表的总大小在10 GB以内。