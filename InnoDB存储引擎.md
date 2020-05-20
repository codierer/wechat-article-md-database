# 1 InnoDB简介

InnoDB是一种高可靠性和高性能的通用存储引擎。在`MySQL 8.0`中，`InnoDB`是默认的`MySQL`存储引擎。除非您配置了其他默认存储引擎，否则发出`CREATE TABLE`不带`ENGINE=` 子句的语句将创建一个`InnoDB`表。

# 2 InnoDB[优势](https://dev.mysql.com/doc/refman/8.0/en/innodb-benefits.html "优势")

- DML操作遵循ACID模型，具有 提交，回滚和恢复功能的事务，保护用户数据；
- 实现行锁定，可提高多用户并发性和性能；
- 数据存储在磁盘上，采用基于主键优化查询，每个InnoDB表都有一个主键索引，提高I/O查询时间；
- InnoDB支持 Foreign key 约束，检查插入，更新和删除操作，确保不会导致表间数据不一致；
- 可以压缩表和关联的索引；
- 可以通过查询INFORMATION_SCHEMA表来监视存储引擎的内部工作情况；

# 3 InnoDB[引擎](https://dev.mysql.com/doc/refman/8.0/en/innodb-introduction.html "引擎")功能
| 特征   | 支持 |
| :----- | :-- |
| B树索引 |  是  |
| 聚集索引 |  是  |
| 压缩数据 |  是  |
| 资料快取 |  是  |
| 外键支持 |  是  |
| 加密数据 |  是  |
| 全文搜索索引 |  是  |
| 地理空间数据类型支持 |  是  |
| 地理空间索引支持 |  是  |
| 索引缓存 |  是  |
| 锁定粒度 |  行  |
| MVCC |  是  |
| 存储限制 |  64TB  |
| 交易次数 |  是  |
| 更新数据字典统计信息 |  是  |
| 集群数据库支持 |  否  |
| 哈希索引 |  否  |
| T树索引 |  否  |


# 4 InnoDB最佳[实践](https://dev.mysql.com/doc/refman/8.0/en/innodb-best-practices.html "实践")

- 为查询频率高的列指定为主键，或者采用自增策略作为主键；
- 使用`join`时,为提高连接性能，为连接列定义外键；
- 关闭自动提交；
- 批量数据进行DML操作时，通过使用`start transaction`和`commit`语句进行执行；
- 不建议使用 `lock tables`， InnoDB可以一次处理多个会话，一次读写同一张表，对一组行的排他性访问，使用`select ... for update` 锁定更新的行；
- 启用innobd_file_per_table选项，将表的数据和索引放入单独的文件中，而不是系统表空间（默认情况是启动状态）。



