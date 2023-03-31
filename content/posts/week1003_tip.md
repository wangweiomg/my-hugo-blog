---
title: "Week1003_tip"
date: 2023-03-31T11:58:52Z
draft: false 
toc: false
images:
tags:
  - arts 
series:
  - arts 
categorys:
  - learn 
---

## ARTS - Tip

### 关于TKMybatis 的一个小技巧
本人在开发一个实体类时候，对这个实体类进行有参构造，但是没有加无参构造，于是在使用tk mapper的查询方法时候， 就一直报错没有 xxx 构造函数，由于 tk 是不用写xml mapper的，问题可能出在对象关系映射那块，猜测tk是想自己根据数据库类型，来转成Java类型的，不需要自己指定参数类型，于是解决办法是再加个无参构造函数就解决了。