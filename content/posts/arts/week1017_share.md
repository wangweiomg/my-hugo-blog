---
title: "Week1017_share"
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
## Java项目的 接口-实现 写法真有必要吗

### 习惯的日常
由于经常做内部系统，所以只讨论在开发内部系统的范围。

经典的MVC架构，经典的Spring、MyBatis 组合，开发内部系统已经是成熟的套路了，不需要思考：

* Controller 接收用户请求
* Service 接口处理用户逻辑
* Service 实现真正处理逻辑，用各种Dao来操作数据库。
* MyBatis 生成各种xml，或者根据需要特别写SQL

然而，这个过程使用久了，觉得就应该是这么写的，但是为什么非要这么写呢？

### 接口的本意和实际使用

软件工程，有很多思想确实很好，面向接口编程，接口和实现分离，开闭原则，高内聚，低耦合，我们使用接口，就是方便扩展、更换接口实现更方便，这样的话接口不需要变动，对客户端来说是无影响的。我承认在duboo这类框架里这么做是有必要的，但是对于开发内部系统， 我们面对最多的就是业务处理，Controller接收用户请求，然后在对应的Service里处理，如果我们因为业务变动或bug等，我们要么直接写一个方法，或者在原来方法上进行调整，当然如果使用了设计模式，有些确实需要接口，但是对于很多业务处理，不需要搞复杂的模式，既然接口在这里用处并不如期望的那样，那么为何不去掉，直接注入Service实现？

### 在Controller里做逻辑处理
更有甚者，在Controller 里做各种逻辑处理，直接使用dao操作数据，理由是写Servcie层没必要。刚开始听到这种说法觉得挺震惊，后来想想确实，只是简单的逻辑，没有事务处理，不需要更多的扩展性，直接在Controller处理又有何不可呢？ 虽然Controller也可以事务处理，虽然没那么优雅，但是却是最简洁高效的。

如果程序中有更多的复用，还是写Service比较好，另外和事务操作分开管理，更优雅，这里依然可以舍弃Service接口。

### 更进一步思考

我们知道，json已经成为了事实上的前后端交互的数据标准，后台做的事是，就是接收json,转为bean, 存数据库，取数据就是把数据封装为bean,在转为json返回前台，既然要这么转，为何后台不直接使用json 来做数据处理，省去了来回转，岂不是更好？

基于这样的思想，有人开发了一个框架，叫 Latke , 参考这篇文章看介绍 [ 为什么又要造一个叫 Latke 的轮子](https://hacpai.com/article/1403847528022).

一句流行的话说，既要埋头搬砖，也要抬头看路，多思考下自己的业务场景和应对手段，也是很有益处的。