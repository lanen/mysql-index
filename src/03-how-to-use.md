# 如何使用索引

前面我们知道有索引的这么一个概念，那么回到今天我们主要的话题，我们如果更好来使用MySQL
的索引机制。

有8个主要目的，来看下 MySQL 官方对索引的使用目的：

1. 加速`WHERE 条件` 的匹配
2. 加速数据定位，排查干扰的数据行。（索引尽量建在辨识度高的列上面）
3. `复合索引`和`最左前缀匹配`规则，优化索引空间
4. 加速`连表`匹配
5. 聚合查询函数优化,如 `MIN()`, `MAX()`
6. 加速分组和排序
7. 覆盖索引
8. 避免全表扫描


我们当前的分享不会覆盖到以上8个方面，仅仅挑出 第1、第3、第4来讲述。
（ppt中，将要讲的点，高亮出来）


进入之前，我们先来了解MySQL 中有哪些索引：

1. 主键索引
2. 唯一索引
3. 单列索引
4. 复合索引

可能没罗列全，但是我们可以围绕这3个问题点来理解：

1. 是否用于组织物理数据
2. 是否对数据有约束作用
3. 是否足够覆盖到关键列
