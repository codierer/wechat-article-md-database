# 1 [Explain](https://blog.csdn.net/eagle89/article/details/80433723 "Explain")执行计划

  `Mysql`在执行`SQL`语句时，如何知道表的读取顺序，读取数据类型操作过程，哪些索引被使用到，这些都对`SQL`调优有着重要的作用.`Explain`作为查看执行计划的重要工具，分析`Explain`结果，对`SQL`调优有重要意义。

下面是使用explain 的例子：
```sql
explain select * from mysql.user;
--query result
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
| id | select_type | table | partitions | type |possible_keys | key  | key_len | ref  | rows | filtered | Extra |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
|  1 |SIMPLE      | user  | NULL      | ALL  | NULL          | NULL | NULL    | NULL |  26 |   100.00 | NULL  |
+----+-------------+-------+------------+------+---------------+------+---------+------+------+----------+-------+
```
其中最重要的字段为：`id`、`type`、`key`、`rows`、`Extra`

> 注：`Explain` 有两个变种：

1. `explain extended`:在`explain`的基础上额外提供一些查询优化的信息.额外有`filtered`列,是一个半分比的值,`rows * filtered/100`可算出,和`explain`中前一个表的连接行数（前一个表指 `explain` 中的id值比当前表id值小的表）。

接着通过`showwarnings`命令得到优化后的查询语句。

2. `explain partitions`:在`explain`的基础上额外提供一些查询优化的信息.额外有`partitions`列,如果查询是基于分区表的话,会显示查询将访问的分区。

# 2 字段详解
## 2.1 id
`id`是理解`SQL`执行的顺利标识,`SQL`的执行顺序由大到小。
1. `id`相同：执行顺序由上至下;
2. `id`不同：如果是子查询，`id`的序号会递增，`id`值越大优先级越高，越先被执行.

## 2.2 select_type
查询的类型，主要是用于区分普通查询、联合查询、子查询等复杂的查询。
1) `SIMPLE`：简单的`select`查询，查询中不包含子查询或者`UNION`;
2) `PRIMARY`：查询中包含任何复杂的子部分，最外层查询则被标记为`primary`;
3) `SUBQUERY`：在`select` 或 `where`列表中包含子查询;
4) `DEPENDENT SUBQUERY`:子查询中的第一个SELECT，取决于外面的查询;
5) `DERIVED`：在`from`列表中包含子查询被标记为`derived`（衍生），`mysql`或递归执行这些子查询;
6) `UNION`：第二个`select`出现在`UNION`之后，则被标记为`UNION`;
7) `DEPENDENT UNION`：`UNION`中的第二个或后面的`SELECT`语句，取决于外面的查询;
8) `UNION RESULT`：`UNION`的结果。
## 2.3 table
值表示 `explain` 的一行正在访问表名。

## 2.4 type
表示关联类型或访问类型，即MySQL决定如何查找表中的行。

访问类型，sql查询优化中一个很重要的指标，结果值从好到坏依次是：
`system` >`const` > `eq_ref` > `ref` > `fulltext` > `ref_or_null` > `index_merge` >`unique_subquery` > `index_subquery` > `range` > `index` > `ALL`

**一般而言,良好的sql查询至少达到range级别，最好能达到ref级别。**
1. `system`：表只有一行记录（等于系统表），这是const类型的特例，平时不会出现，可以忽略不计；

2. `const`：表示通过索引一次就找到了，const用于比较primary key 或者 unique索引。因为只需匹配一行数据，所有很快。如果将主键置于where列表中，mysql就能将该查询转换为一个const

3. `eq_ref`：唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见于主键或 唯一索引扫描。=》primarykey 或 unique key 索引的所有部分被连接使用，最多只会返回一条符合条件的记录。这可能是在 const 之外最好的联接类型了，简单的 select 查询不会出现这种 type。

注意：ALL全表扫描的表记录最少的表如t1表

4. `ref`：相比 eq_ref，不使用唯一索引，而是使用普通索引或者唯一性索引的部分前缀，索引要和某个值相比较，可能会找到多个符合条件的行。

5. `ref_or_null`：类似ref，但是可以搜索值为NULL的行。

6. `index_merge`：表示使用了索引合并的优化方法。 例如下表：id是主键，tenant_id是普通索引。or 的时候没有用 primary key，而是使用了 primary key(id) 和 tenant_id 索引.

7. `range`：范围扫描通常出现在 in(), between ,> ,<, >= 等操作中。使用一个索引来检索给定范围的行。

8. `index`：Full Index Scan，index与ALL区别为index类型只遍历索引树。这通常为ALL块，应为索引文件通常比数据文件小。（Index与ALL虽然都是读全表，但index是从索引中读取，而ALL是从硬盘读取）

9. `ALL`：Full Table Scan，遍历全表以找到匹配的行

## 2.5 possible_keys
这列显示查询可能使用哪些索引来查找。
## 2.6 key
这列显示`mysql`实际采用哪种索引，从而进一步优化对该表的访问。
## 2.7 key_len
表示索引中使用的字节数，查询中使用的索引的长度（最大可能长度），并非实际使用长度，理论上长度越短越好。
## 2.8 ref
显示索引的那一列被使用了，如果可能，是一个常量`const`。

这一列显示了在`key`列记录的索引中，表查找值所用到的列或常量，常见的有：`const`（常量），`func`，`NULL`，字段名（例：film.id）
## 2.9 rows
根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数

## 2.10 Extra

- `Using filesort`:MySQL有两种方式可以生成有序的结果，通过排序操作或者使用索引，当`Extra`中出现了`Using filesort` 说明`MySQL`使用了后者，但注意虽然叫`filesort`但并不是说明就是用了文件来进行排序，只要可能排序都是在内存里完成的。大部分情况下利用索引排序更快，所以一般这时也要考虑优化查询了。使用文件完成排序操作，这是可能是ordery by，`group by`语句的结果，这可能是一个CPU密集型的过程，可以通过选择合适的索引来改进性能，用索引来为查询结果排序。

- `Using temporary`:用临时表保存中间结果，常用于`GROUP BY` 和 `ORDER BY`操作中，一般看到它说明查询需要优化了，就算避免不了临时表的使用也要尽量避免硬盘临时表的使用。

- `Not exists`:`MYSQL`优化了`LEFT JOIN`，一旦它找到了匹配`LEFT JOIN`标准的行， 就不再搜索了。

- `Using index`:说明查询是覆盖了索引的，不需要读取数据文件，从索引树（索引文件）中即可获得信息。如果同时出现`using where`，表明索引被用来执行索引键值的查找，没有`using where`，表明索引用来读取数据而非执行查找动作。这是MySQL服务层完成的，但无需再回表查询记录。

- `Using index condition`:这是MySQL 5.6出来的新特性，叫做“索引条件推送”。简单说一点就是MySQL原来在索引上是不能执行如like这样的操作的，但是现在可以了，这样减少了不必要的IO操作，但是只能用在二级索引上。

- `Using where`:使用了`WHERE`从句来限制哪些行将与下一张表匹配或者是返回给用户。注意：`Extra`列出现`Using where`表示`MySQL`服务器将存储引擎返回服务层以后再应用WHERE条件过滤。

- `Using join buffer`:使用了连接缓存：`Block Nested Loop`，连接算法是块嵌套循环连接;`Batched Key Access`，连接算法是批量索引连接

- `impossible where`:`where`子句的值总是`false`，不能用来获取任何元组

- `select tables optimized away`:在没有`GROUP BY`子句的情况下，基于索引优化`MIN/MAX`操作，或者对于`MyISAM`存储引擎优化`COUNT(*)`操作，不必等到执行阶段再进行计算，查询执行计划生成的阶段即完成优化。

- `distinct`:优化`distinct`操作，在找到第一匹配的元组后即停止找同样值的动作

