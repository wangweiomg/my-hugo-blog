---
title: "Week1025_share"
date: 2023-03-31T15:45:41Z
draft: false 
toc: false
images:
categorys:
  - 每周学习
series:
tags:
  - arts 
---

## ARTS - Share  - 补12.24日
## MySQL的联合索引

### 最左匹配原则

索引的重要性就不再说了，反正使用数据库的都知道，我们来说说联合索引。说到联合索引，就要说它的最左匹配原则，由于B+Tree 的特点，索引从左往右建，加入一张表 table(col1, col2, col3), 建立联合索引  create index idx_table_col1_col2 on table(col1, col2);

在查询时候   where col1 = xxx and col2 = xxx  和  where col1 ，都会用到这个联合索引，但是如果 where col2 = xxx ， 这种情况是用不到联合索引的。


### 实际案例

本人在进行一个百万级表的查询时候，碰到了这个问题。

在使用 where create_date = xxxx时候，
使用执行计划查看：

```

+----+-------------+-------+------------+--------+-----------------------------+---------+---------+-------------------+---------+----------+----------------------------------------------+
| id | select_type | table | partitions | type   | possible_keys               | key     | key_len | ref               | rows    | filtered | Extra                                        |
+----+-------------+-------+------------+--------+-----------------------------+---------+---------+-------------------+---------+----------+----------------------------------------------+
| 1  | SIMPLE      | d     | <null>     | ALL    | idx_create_by | <null>  | <null>  | <null>            | 2343409 |   1.23   | Using where; Using temporary; Using filesort |
| 1  | SIMPLE      | c     | <null>     | eq_ref | PRIMARY                     | PRIMARY | 122     | d.apply_id  | 1       | 100.0    | Using index                                  |
| 1  | SIMPLE      | u     | <null>     | eq_ref | PRIMARY                     | PRIMARY | 194     | d.create_by | 1       | 100.0    | Using where                                  |
+----+-------------+-------+------------+--------+-----------------------------+---------+---------+-------------------+---------+----------+----------------------------------------------+
```

触发了全表扫描，而且，使用 show index from table; 查看索引情况：

```
+---------------------+------------+-----------------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| Table               | Non_unique | Key_name                    | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment |
+---------------------+------------+-----------------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
| table | 0          | PRIMARY                     | 1            | id          | A         | 2342964     | <null>   | <null> |      | BTREE      |         |               |
| table | 0          | table_unique        | 1            | apply_id    | A         | 426585      | <null>   | <null> |      | BTREE      |         |               |
| table | 0          | table_unique        | 2            | create_date | A         | 2296706     | <null>   | <null> |      | BTREE      |         |               |
| table | 1          | table_groupId       | 1            | group_id    | A         | 10          | <null>   | <null> | YES  | BTREE      |         |               |
| table | 1          | table_time          | 1            | pay_time    | A         | 409528      | <null>   | <null> | YES  | BTREE      |         |               |
| table | 1          | table_time          | 2            | create_date | A         | 414810      | <null>   | <null> |      | BTREE      |         |               |
| table | 1          | idx_create_by | 1            | create_by   | A         | 272         | <null>   | <null> |      | BTREE      |         |               |
+---------------------+------------+-----------------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+
```

发现create_date 已经建立了索引，但是为什么查询计划里还是全表扫描呢？

最后使用 show create table  `table` ; 来查看，算是看清楚了 该表使用了 (pay_time, create_date) 来作为联合索引，而 show index 里面显示的两个同名key_name ，其实就是联合索引的意思，怪我没有看仔细，漏了这个重要线索。


然后解决起来就是方便了，给create_date 建立单独的索引，然后查询时间从半分钟降到1秒以内。


### 总结

看题要仔细，不然很容易忽略细节，然后就容易南辕北辙，以为自己目前掌握的知识不足以解决，产生畏惧心理。

基础要打牢，最左匹配原则要结合实际案例来记忆，不然浮于表面，无法形成有效知识。



