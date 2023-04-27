---
title: "Java面试中常问的问题"
date: 2023-04-12T03:37:21Z
draft: false
toc: false
images:
tags:
  - Java
  - 面试
---
## Java面试中常问的问题

1. Java HashMap 两次put会发生什么
2. springboot的starter是如何实现自动装配的？
3. redis为什么快？
4. redis常用的数据结构是什么，
5. redis如何实现分布式锁，
6. redis实现分布式锁有什么问题
7. redis如果系统错误释放锁和业务执行超时释放锁，会发生什么？
8. 分布式事务如何保证一致性
9. 服务发现是怎么回事
10. kafka消息队列为何要引入组？解决了什么问题
11. mysql ABC索引， 实际用到了AB，没用到C，是什么情况会发生？



### 1. Java HashMap 两次put会发生什么

我们要了解HashMap的结构，并了解put时候发生了什么。

HashMap是键值对结构， 计算key的hash值来找对对应桶的位置，然后放进去。所以put(key, value) 基本可以认为 先 hash(key), 找到位置，放进去，再put同样的 key,  位置是一样的， 那就把原有值覆盖掉。

如果是put的是不同的key， hash(key)后找到桶位置，如果桶里没有值，直接就放进去，有的话用equals一个个比较，找到就替换值，找不到就拼到链表结尾。



### 2. springboot的starter是如何实现自动装配的？

我们知道springboot由于有很多starter和注解的存在大大简化了spring的使用，减少了很多xml配置，那么它是如何做到的？

其实是由springboot自定义starter的配置来完成的，springboot 通过在WEB-INF目录下spring.factories 配置自己定义注解，然后引入到工程里。  在2.7.11 里， [`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`](https://github.com/spring-projects/spring-boot/tree/v2.7.11/spring-boot-project/spring-boot-autoconfigure/src/main/resources/META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports) 改这个文件就可以达到相同的结果。



### 3. redis为什么快？

1. 数据结构简单：Redis中的数据结构相对简单，只包含基本的数据结构，例如字符串、列表、集合、哈希表和有序集合等，相比其他的数据库，Redis需要更少的时间用于数据结构实现和维护。

2. 内存操作：Redis是高速的内存数据库，数据存储在内存中，所有操作都是内存操作，不存在硬盘IO等慢速操作。

3. 单线程模型：Redis采用单独的IO线程，使得应用程序并发访问时需要的锁或者同步操作少很多，提高了系统的吞吐量。

4. 异步非阻塞IO：Redis采用异步非阻塞IO模型，使得服务器能够处理成百上千个客户端的请求，而且不会由于单个客户端请求阻塞而导致服务器无法处理其他请求。

综上所述，Redis之所以快就是因为其采用了简单的数据结构、内存操作、单线程模型和异步非阻塞IO等技术，从而大幅提升了数据读写的速度和系统吞吐量。

### 4. redis数据结构有哪些

1. 字符串（Strings）：存储字符串、整数或者浮点数。

2. 列表（Lists）：一个列表结构，通过一个键可以对一个字符串列表进行操作，支持从列表两端（左端/右端）添加或者删除数据。

3. 集合（Sets）：一个无序的、元素不重复的集合，支持对集合进行添加、删除和判断两个集合的交集、并集和差集等操作。

4. 哈希表（Hashes）：一个键值对的集合，键和值都是字符串类型，支持添加、修改、删除和查询具体的某个键值对。

5. 有序集合（Sorted Sets）：与集合类似，是一个无序的、元素不重复的集合，但不同的是，每一个元素都会附带一个权重（score），支持根据权重快速查询元素。

其中，字符串、列表、集合、哈希表和有序集合都具有很好的应用场景，应用广泛，并且在实现上都相对简单，易于理解和使用。

### 5. 如何用redis实现分布式锁

redis的原子操作 setnx来实现



