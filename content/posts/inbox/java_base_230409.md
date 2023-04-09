---
title: "Java基础之集合"
date: 2023-04-09T02:51:02Z
draft: false
toc: false
images:
series:
  - Java基础
tags:
  - 面试
  - Java
---
## Java基础之集合总览

Java集合是经常使用到的，集合类主要由Collection 和 Map 这两个接口派生。

Collection 派生出 Set，List, Queue。 Map派生HashMap, TreeMap， ConcurrentHashMap

### Collection常用方法

```java
// 返回集合中元素个数
int size();

//Returns true if this collection contains no elements.
boolean isEmpty();

// 判断集合中是否存在元素o , (o==null ? e==null : o.equals(e)).
boolean contains(Object o);

//  添加元素
boolean add(E e);

// 删除元素
boolean remove(Object o);

// 清空元素
void clear();

```

### Map常用方法

```java


// 返回 k-v 数量
int size();

// Returns <tt>true</tt> if this map contains no key-value mappings.
boolean isEmpty();

// 存在key就返回true,
boolean containsKey(Object key);

boolean containsValue(Object value);

// 放入k,v ， key可以为null
V put(K key, V value);

// 删除k-v,  key可以为null 
V remove(Object key);

// 返回key集合
Set<K> keySet();

// 返回key-value 集合
Set<Map.Entry<K, V>> entrySet();

// 返回value集合
Collection<V> values();

// equals 比较
// (e1.getKey()==null ? e2.getKey()==null : e1.getKey().equals(e2.getKey()))  
//	&&
//   (e1.getValue()==null ? e2.getValue()==null : e1.getValue().equals(e2.getValue()))
boolean equals(Object o);

// hashcode
// (e.getKey()==null   ? 0 : e.getKey().hashCode()) ^ (e.getValue()==null ? 0 : e.getValue().hashCode())
int hashCode();

```
