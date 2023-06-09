---
title: "Week1026_tip"
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

## ARTS - Tip 补 2019.1.2
## 多人查询系统加锁

### 问题
由于有几百人同时查询一个借口，造成数据库压力飙升，在优化完数据库索引、缓存之后，发现了多人查询返回的结果是一样的，然后在系统中做过滤，可以根据这一特点进行优化。


### 思路

因为多人请求返回同样的数据，那么就可以让第一个人请求数据库，返回后放入缓存，其他人从缓存中拿，如果在请求中出现了另一个人查询，直接返回空。

### 实现

刚开始使用了AtomicInteger  原子整形来实现，通过和初始化值做对比，如果没被修改过，就相当于获得了锁，把他改变值，然后去查询db, 存入缓存。

后来发现，如果其他参数的用户请求数据，那么也会因此而被阻塞，所以就使用了集合，以参数为key, 存入集合，如果存在该key,那么相当于没有获得锁，当然集合要线程安全，使用了ConcurrentLinkedQueue来实现。

### 总结
上线之后，性能提升明显。 优化系统不仅从技术层考虑： 优化索引、加缓存， 还要从系统业务角度入手，优化哪些不需要的流程，进一步提高系统性能。

### 隐忧

虽然通过加锁方式提高了性能， 但是目前是单机应用，当用户量进一步提升，势必会改成多机分布式应用，到时候这种加锁方式就需要改造成分布式锁来实现了。

