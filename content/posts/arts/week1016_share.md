---
title: "Week1016_share"
date: 2023-03-31T15:45:37Z
draft: false 
toc: false
images:
categorys:
  - 每周学习
series:
tags:
  - arts 
---

## ARTS - Share
## 关于SQL JOIN 的那些事

### 一张图
SQL中关联查询使用最多的是各种JOIN，这个图中简单明了的说明了各个JOIN 的含义：

![](https://i.stack.imgur.com/1UKp7.png)

### 测试数据
准备了两张表 ``` t_user 和 t_info```, 数据如下：

```
mysql> select * from t_user;
+----+------+------+-------------+
| id | name | age  | telephone   |
+----+------+------+-------------+
|  1 | jack |   20 | 18877771111 |
|  2 | rose |   33 | 18977771111 |
|  3 | tom  |   21 | 13877761111 |
|  4 | bob  |   24 | 13356771111 |
+----+------+------+-------------+

mysql> select * from t_info;
+----+---------+-----------------+--------------+
| id | user_id | bank_no         | company_name |
+----+---------+-----------------+--------------+
|  1 |       1 | 110132011114578 | google       |
|  2 |       2 | 110119677118521 | facebook     |
|  3 |       3 | 110111842716547 | twitter      |
|  4 |       8 | 210111842716547 | baidu        |
|  5 |       9 | 410111842716547 | tencent      |
+----+---------+-----------------+--------------+

```


### 1. LEFT JOIN

根据LEFT JOIN 图，我们知道，数据返回 A 表所有数据 + B表和A相交的部分, 空的部分为NULL。

```

SELECT * FROM t_user a LEFT JOIN t_info b ON a.id = b.user_id;

mysql> SELECT * FROM t_user a LEFT JOIN t_info b ON a.id = b.user_id;
+----+------+------+-------------+------+---------+-----------------+--------------+
| id | name | age  | telephone   | id   | user_id | bank_no         | company_name |
+----+------+------+-------------+------+---------+-----------------+--------------+
|  1 | jack |   20 | 18877771111 |    1 |       1 | 110132011114578 | google       |
|  2 | rose |   33 | 18977771111 |    2 |       2 | 110119677118521 | facebook     |
|  3 | tom  |   21 | 13877761111 |    3 |       3 | 110111842716547 | twitter      |
|  4 | bob  |   24 | 13356771111 | NULL |    NULL | NULL            | NULL         |
+----+------+------+-------------+------+---------+-----------------+--------------+
 
```

再多说一句 用 left join 之后的 and 和  where 过滤的问题， and会过滤 b表的数据，对整体结果不影响， where 会过滤整体，如下：

```
mysql> SELECT * FROM t_user a LEFT JOIN t_info b ON a.id = b.user_id where b.id <3;
+----+------+------+-------------+------+---------+-----------------+--------------+
| id | name | age  | telephone   | id   | user_id | bank_no         | company_name |
+----+------+------+-------------+------+---------+-----------------+--------------+
|  1 | jack |   20 | 18877771111 |    1 |       1 | 110132011114578 | google       |
|  2 | rose |   33 | 18977771111 |    2 |       2 | 110119677118521 | facebook     |
+----+------+------+-------------+------+---------+-----------------+--------------+


mysql> SELECT * FROM t_user a LEFT JOIN t_info b ON a.id = b.user_id and  b.id <3;
+----+------+------+-------------+------+---------+-----------------+--------------+
| id | name | age  | telephone   | id   | user_id | bank_no         | company_name |
+----+------+------+-------------+------+---------+-----------------+--------------+
|  1 | jack |   20 | 18877771111 |    1 |       1 | 110132011114578 | google       |
|  2 | rose |   33 | 18977771111 |    2 |       2 | 110119677118521 | facebook     |
|  3 | tom  |   21 | 13877761111 | NULL |    NULL | NULL            | NULL         |
|  4 | bob  |   24 | 13356771111 | NULL |    NULL | NULL            | NULL         |
+----+------+------+-------------+------+---------+-----------------+--------------+

```


这时候就能获取 A B相交， A中独有的部分， 用where 把 b 连接ID不为空的过滤掉，如下：

```
mysql> SELECT * FROM t_user a LEFT JOIN t_info b ON a.id = b.user_id  where b.user_id is null;
+----+------+------+-------------+------+---------+---------+--------------+
| id | name | age  | telephone   | id   | user_id | bank_no | company_name |
+----+------+------+-------------+------+---------+---------+--------------+
|  4 | bob  |   24 | 13356771111 | NULL |    NULL | NULL    | NULL         |
+----+------+------+-------------+------+---------+---------+--------------+
```

### 2. RIGHT JOIN

又连接，正好反过来，直接看例子吧：

```
mysql> select * from t_user u right join t_info i on i.user_id = u.id;
+------+------+------+-------------+----+---------+-----------------+--------------+
| id   | name | age  | telephone   | id | user_id | bank_no         | company_name |
+------+------+------+-------------+----+---------+-----------------+--------------+
|    1 | jack |   20 | 18877771111 |  1 |       1 | 110132011114578 | google       |
|    2 | rose |   33 | 18977771111 |  2 |       2 | 110119677118521 | facebook     |
|    3 | tom  |   21 | 13877761111 |  3 |       3 | 110111842716547 | twitter      |
| NULL | NULL | NULL | NULL        |  4 |       8 | 210111842716547 | baidu        |
| NULL | NULL | NULL | NULL        |  5 |       9 | 410111842716547 | tencent      |
+------+------+------+-------------+----+---------+-----------------+--------------+


-- 查 b 表独有的

mysql> select * from t_user a right join t_info b on b.user_id = a.id where a.id is null;
+------+------+------+-----------+----+---------+-----------------+--------------+
| id   | name | age  | telephone | id | user_id | bank_no         | company_name |
+------+------+------+-----------+----+---------+-----------------+--------------+
| NULL | NULL | NULL | NULL      |  4 |       8 | 210111842716547 | baidu        |
| NULL | NULL | NULL | NULL      |  5 |       9 | 410111842716547 | tencent      |
+------+------+------+-----------+----+---------+-----------------+--------------+
```


### INNER JOIN 

这个是查两个表的交集, 两边都匹配上

```
mysql> select * from t_user a inner join t_info b on b.user_id = a.id ;
+----+------+------+-------------+----+---------+-----------------+--------------+
| id | name | age  | telephone   | id | user_id | bank_no         | company_name |
+----+------+------+-------------+----+---------+-----------------+--------------+
|  1 | jack |   20 | 18877771111 |  1 |       1 | 110132011114578 | google       |
|  2 | rose |   33 | 18977771111 |  2 |       2 | 110119677118521 | facebook     |
|  3 | tom  |   21 | 13877761111 |  3 |       3 | 110111842716547 | twitter      |
+----+------+------+-------------+----+---------+-----------------+--------------+
```

### 3. FULL OUTER JOIN 

要查 AB表的并集：

```
// mysql 不支持全连接，所以只能用 left join union right join 了

mysql> select * from t_user u left join t_info i on i.user_id = u.id
    -> union
    -> select * from t_user u right join t_info i on i.user_id = u.id;
+------+------+------+-------------+------+---------+-----------------+--------------+
| id   | name | age  | telephone   | id   | user_id | bank_no         | company_name |
+------+------+------+-------------+------+---------+-----------------+--------------+
|    1 | jack |   20 | 18877771111 |    1 |       1 | 110132011114578 | google       |
|    2 | rose |   33 | 18977771111 |    2 |       2 | 110119677118521 | facebook     |
|    3 | tom  |   21 | 13877761111 |    3 |       3 | 110111842716547 | twitter      |
|    4 | bob  |   24 | 13356771111 | NULL |    NULL | NULL            | NULL         |
| NULL | NULL | NULL | NULL        |    4 |       8 | 210111842716547 | baidu        |
| NULL | NULL | NULL | NULL        |    5 |       9 | 410111842716547 | tencent      |
+------+------+------+-------------+------+---------+-----------------+--------------+





```

那么， AB交集在 AB并集的补集就是：


```

mysql> select * from t_user a left join t_info b on a.id = b.user_id where b.user_id is null
    -> union
    -> select * from t_user a right join t_info b on a.id = b.user_id where a.id is null;
+------+------+------+-------------+------+---------+-----------------+--------------+
| id   | name | age  | telephone   | id   | user_id | bank_no         | company_name |
+------+------+------+-------------+------+---------+-----------------+--------------+
|    4 | bob  |   24 | 13356771111 | NULL |    NULL | NULL            | NULL         |
| NULL | NULL | NULL | NULL        |    4 |       8 | 210111842716547 | baidu        |
| NULL | NULL | NULL | NULL        |    5 |       9 | 410111842716547 | tencent      |
+------+------+------+-------------+------+---------+-----------------+--------------+
```