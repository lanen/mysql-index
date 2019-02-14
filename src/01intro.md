# MySQL 索引相关

## 基础准备

TODO 数据库安装，测试数据准备，基本的术语

## MySQL 是如何使用索引

`索引`是加速定位数据手段。不使用`索引`的话，定位表数据可能将从第一行开始逐行扫描，随表数据量增大，性能将明显下降，直到无法接受。使用索引，尽量避免全表扫描。

那么MySQL使用索引的目的：

1. 加速`WHERE 条件` 的匹配
2. 加速数据定位，排查干扰的数据行。（索引尽量建在辨识度高的列上面）
3. `复合索引`和`最左前缀匹配`规则，优化索引空间
4. 加速`连表`匹配
5. 聚合查询函数优化,如 `MIN()`, `MAX()`
6. 加速分组和排序
7. 覆盖索引
8. 避免全表扫描

MySQL在定义表的时候，常见索引类型：`PRIMARY KEY`, `UNIQUE`, `INDEX`, `FULLTEXT`。

### 主键索引

The primary key for a table represents the column or set of columns that you use in your most vital queries. It has an associated index, for fast query performance. Query performance benefits from the NOT NULL optimization, because it cannot include any NULL values. With the InnoDB storage engine, the table data is physically organized to do ultra-fast lookups and sorts based on the primary key column or columns.

If your table is big and important, but does not have an obvious column or set of columns to use as a primary key, you might create a separate column with auto-increment values to use as the primary key. These unique IDs can serve as pointers to corresponding rows in other tables when you join tables using foreign keys.

在InnoDB，表数据跟主键索引物理组织在一起，决定数据行存在物理层文件的位置。

### 外键索引

If a table has many columns, and you query many different combinations of columns, it might be efficient to split the less-frequently used data into separate tables with a few columns each, and relate them back to the main table by duplicating the numeric ID column from the main table. That way, each small table can have a primary key for fast lookups of its data, and you can query just the set of columns that you need using a join operation. Depending on how the data is distributed, the queries might perform less I/O and take up less cache memory because the relevant columns are packed together on disk. (To maximize performance, queries try to read as few data blocks as possible from disk; tables with only a few columns can fit more rows in each data block.)

相关性高的列聚合在一个表，通过外键与主表建立管理。好处是有效减少io，因为读出来的都是相关数据。

### 单列索引

![image](./images/chapter01-intorl.png)


在列上面建立索引，会将拷贝列的值存在索引上面。

B-Tree 索引能够提供快速的单值匹配，范围集合匹配。 在Where 子句上面，列与 `=, >, ≤, BETWEEN, IN` 配合使用。

The maximum number of indexes per table and the maximum index length is defined per storage engine

每个表能够建立索引个数，由存储引擎决定。一般在至少16个索引，索引长度 在 256 字节内。

* 前缀索引：可以给 `CHAR, VARCHAR, TEXT` 类型的列建立前缀索引，colname(N), N表示字符长度。可以 `BINARY, VARBINARY` 类型的列建立前缀索引，colname(N),表示字节数。
* 全文索引： 针对列的全部数据，不支持前缀索引。

### 复合索引

复合索引，支持组合16个索引。

### explain 验证索引是否生效

explain 支持： `SELECT, DELETE, INSERT, REPLACE, and UPDATE`

Explain 呈现 MySQL 对查询语句的执行计划。

explain 的输出格式：

id: 查询 select 语句的id

select_type: 查询的类型

Table: 查询的是哪个表

partitions： 匹配到哪个分区上

type: access_type, join 连表类型

possible_key： 可能选择使用哪些索引

Key： 实际选择的索引

key_len： 索引的长度

ref: 哪些列匹配了索引

Rows： 预计要匹配多少行

filtered: 覆盖率？

extra： 

#### 输出格式列的可能取值内容

 ALL, index,  range, ref, eq_ref, const, system, NULL（从左到右，性能从差到好）
 
type: 表被访问的方式。 有 system、const、eq_ref、ref、

走索引PRIMARY KEY or UNIQUE会 const

https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#explain-join-types

eq_ref 和 ref 怎么理解？

eq_ref : 使用 PRIMARY KEY or UNIQUE NOT NULL进行匹配连表，单行
ref: 简单理解成非 eq_ref，有多个值匹配到的。

Fulltext: 全文索引

ref_or_null : 很像ref，但是增加了 OR column is null 查询支持

index_merge : 将多个索引命中合并成1个。

range: can be used  `=, <>, >, >=, <, <=, IS NULL, <=>, BETWEEN, LIKE, or IN()`:


unique_subquery: This type replaces eq_ref for some IN subqueries of the following form:



 


#### select_type

SIMPLE : 简单的查询，没有子查询或者UNION等
PRIMARY: 最外层的查询

### Index hints

MySQL 如何选择索引？

### Explain Extra

semi-join LooseScan strategy 


### 索引的原理



	
## 场景


## 算法



各种`存储引擎`对这些索引类型的支持情况：

`InnoDB` 使用`B-trees` 存储索引。

材料准备：

https://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_b_tree

https://dev.mysql.com/doc/refman/5.7/en/index-btree-hash.html



想了解当前数据库示例支持的存储引擎，可以使用命令 `SHOW ENGINES;` 得到类似下表：

# 优化篇

* 数据库级别
* 硬件级别
* 负载

## 数据库级别

* 表结构设计是否合理
* 索引是否正确
* 存储引擎是否选择合理
* 行格式是否合理
* 锁策略是否合理
* 缓存是否合理

### SQL 优化

* Select statement
* 子查询、视图
* information_schema query
* data change statements
* data privilege

### Database structure

* data size
* data types
* Optimizing for Many Tables
* Internal Temporary Table Use in MySQL


### InnoDB 优化

 Optimizing Storage Layout for InnoDB Tables
8.5.2 Optimizing InnoDB Transaction Management
8.5.3 Optimizing InnoDB Read-Only Transactions
8.5.4 Optimizing InnoDB Redo Logging
8.5.5 Bulk Data Loading for InnoDB Tables
8.5.6 Optimizing InnoDB Queries
8.5.7 Optimizing InnoDB DDL Operations
8.5.8 Optimizing InnoDB Disk I/O
8.5.9 Optimizing InnoDB Configuration Variables
8.5.10 Optimizing InnoDB for Systems with Many Tables

## 硬件级别

* 磁盘寻址速度
* 磁盘读写速度
* CPU 性能
* 内存带宽

## 阅读分析


8.3 Optimization and Indexes
8.3.1 How MySQL Uses Indexes
8.3.2 Primary Key Optimization
8.3.3 Foreign Key Optimization
8.3.4 Column Indexes
8.3.5 Multiple-Column Indexes
8.3.6 Verifying Index Usage
8.3.7 InnoDB and MyISAM Index Statistics Collection
8.3.8 Comparison of B-Tree and Hash Indexes
8.3.9 Use of Index Extensions
8.3.10 Optimizer Use of Generated Column Indexes

1. MySQL 如何使用索引
2. 主键索引优化
3. 外键索引优化
4. 列索引
5. 多列索引
6. 验证索引的用法
7. 比较 B-Tree  和 Hash 索引
8. 索引的扩展
9. 索引优化器