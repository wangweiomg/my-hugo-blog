---
title: "Week1015_review"
date: 2023-03-31T15:45:36Z
draft: false 
toc: false
images:
categorys:
  - 每周学习
series:
tags:
  - arts 
---

## ARTS - Review
## [mysql性能优化](https://codeburst.io/database-performance-optimization-8d8407808b5b)

数据库性能优化最重要的事是选择正确的方式。你应该决定你的应用需要关系型或非关系型数据库。甚至里面的一个类型，你都会有好几个选择。在关系型数据库里，你可以选择Oracle， MySQL, SQL Server,和PostgreSQL. 另一方面，非关系型数据库例如MongoDB, Cassandra和CouchDB. 你可能希望我建议用读写性能快的非关系型数据库。然而，通过一些改进和调整，你可以把关系型数据推到超过它已知的限制。所以，在本文中，我将会介绍给你一些技巧来让你的MySQL数据库更快。如果你想知道为什么特别是MySQL, 那么答案是简单的，因为它免费，开源在PHP社区很流行Oracle 并没有广泛使用因为它很昂贵。其他的选择并没有像MySQL这么受欢迎。

### MySQL服务配置
首先你应该知道根据你的操作系统配置文件是在哪里。在Linux系统，它在 “/etc/mysql/my.cnf”.
是时候拿上你的引擎对比InnoDB和MyISAM了。为了让选择更简单，你应该知道InnoDB成为默认引擎始于MySQL5.5 因为它支持“行级锁， 外键和事务” ，MyISAM不支持这里提到的任何在现实应用中很少使用的特性。

选择正确的引擎后，是时候设置一些在my.cnf 文件里的配置变量。

#### ```max_connection``` 变量:

```max_connection``` 变量代表你的应用允许的连接数量。默认是151个连接，然而你可以轻松增加这个数量如果你获得这样一个错误：“MySQL ERROR , Too many connections...”

```max_connections``` = 170

#### ```innodb_buffer_pool_size``` 变量：
为了加速，MySQL 将会缓存数据在你的服务器内存，这个变量告诉MySQL能用多少GB。这个变量在你的数据库里存储大的blobs时候非常有用。你可以设置为你服务器80%，90%内存大小。所以你的服务器内存是16GB， 你可以设置它为14GB.

```innodb_buffer_pool_size``` = 14GB


#### ```innodb_io_capacity``` 变量：

这个变量告诉MySQL它可以用多少操作系统IO，它受制于你的硬盘。例如，一个单个7200 RPM 驱动限制200 I/O 一个企业级SSD硬盘是 50000 I/O. 你可以通过你操作系统的命令行轻松发现IO值和设置变量为可用IO的90% ， 这样mysql将会使用太多的IO操作。


```innodb_io_capccity=21000```

#### ```query_cache_limit 和 query_cache_size ```变量
MySQL的缓存也在内存里，然而我们不能把它当做一个缓存系统，每次你的应用写入一个数据库表，MySQL将会重置查询缓存为全部表。所以，如果你的应用有一个高比例负载，MySQL 缓存将会完全无用，在那种情况下，最好把这两个参数设置为0来节省管理MySQL缓存的开销，另外你可以用一些像Redis这样的管理缓存。

```
query_cache_limit = 0
query_cache_size = 0
```

#### 慢查询日志
慢查询日志将会展示哪个个查询花费比设置更长的时间，不需要猜测哪个查询慢。首先，你需要启用慢查询日志在你的配置文件。在Linux上，打开/etc/mysql/my.cnf 或者同样的在你系统里加入：

```
slow_query_log = 1
long_query_time = 1
```

好了，这两个选项将会开启慢查询日志将会记录任何超过1秒的查询。如果你更喜欢在表里查看日志而不是在文件里，你可以添加：

```
log_output = ‘TABLE’
```

然后你就可以在一个叫 “slow_log” 的表中查看日志，你将会看到所有关于花费超过1秒的执行查询的信息。信息包括执行查询的时间有多少行命中哪个用户执行的它。

#### 查询优化

你获得所有慢查询之后，你需要寻找一种方法优化他们让他们变得更快。所以你可以使用"explain"在查询之前来获得查询的详细信息。例如： “explain select * from users where active=1;”

explain 关键字能帮你定位哪个索引被你的查询命中和你想要查询的数据的行数。这个信息能告诉你是否需要创建更多的索引或者重新组织你的数据库表。


#### 非规范化和约束

非规范化是通过添加冗余数据和分组来提高读性能的处理方式。例如，如果你有“product” 表和”category“表，每次查询你都需要获得每个商品的 category_name 。这个例子中，你可能会使用”join“ 来取得 ”category_name“。 然而这意味着一个用户每次都会打开一个商品页面，一个复杂的连接查询将会被执行。所以你可以考虑将”category_name“ 添加到product表。尽管有冗余数据，但是提升了读性能，还是值得的。


非规范方法可能导致 product表中的 category_name 过时，所以你需要定义一个外键”foreign key“ 约束，但是你需要了解一个 外键将会让写性能慢了一点，因为MySQL需要在写数据之前检查约束。所以它也常常是你的工作来决定怎样才是提高数据性能的最好选择。

