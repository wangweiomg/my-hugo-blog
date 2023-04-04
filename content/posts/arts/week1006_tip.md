---
title: "Week1006_tip"
date: 2023-03-31T15:45:32Z
draft: false 
toc: false
images:
categorys:
  - 每周学习
series:
tags:
  - arts 
---

## ARTS - Tip

## spring boot多数据源问题

我们知道，在使用多个数据源时候，需要注册到spring boot 里，然后使用时候一般是两种

1. 查询非默认数据源数据时候，加上数据库名前缀
2. 在service 方法里指定数据源，一般是@DataSource("xxx")


在网上很多例子上都是本地环境下两个数据库，但是在开发中需要访问的数据库分布在不同的服务器上，在使用 前缀 + 表 的查询方式不再生效了，就使用了第 2 种方法才生效，注意这点。