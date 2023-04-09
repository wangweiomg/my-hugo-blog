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


### 常用的集合

日常开发最常用最基础的集合：

* ArrayList、LinkedList
* HashMap、ConcurrentHashMap

首先看ArrayList

#### ArrayList

首先看声明

```java

public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{}
```

ArrayList继承了AbstractList ,实现了 List等接口。


```java
// 默认初始容量
private static final int DEFAULT_CAPACITY = 10;

// 共享的空数组实例
private static final Object[] EMPTY_ELEMENTDATA = {};

// 默认容量的空数组实例。我们用这个区分 EMPTY_ELEMENTDATA, 是为了弄清楚首个元素添加时膨胀多少
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

// 数组缓存存入的元素。这个数组的长度就是它的容量。任何等于DEFAULTCAPACITY_EMPTY_ELEMENTDATA的空数组添加首个元素时会被扩展为默认长度
transient Object[] elementData; // non-private to simplify nested class access

// 包含几个元素
private int size;

```

以上几个初始定义，就是ArrayList 经常用的东西， 长度保存在size 里， 元素保存在 Object[] elementData。 我们这里知道ArrayList 是由数组实现的。

继续看构造方法。

```java

public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

```

当你用 new ArrayList() 时候， ArrayList 内部发生的就是把 elementData 初始化为 DEFAULTCAPACITY_EMPTY_ELEMENTDATA; 也就是把空对象数组实例给它，此时它的容量也初始为10。


```java
public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
```

使用如  new ArrayList(50) 时候， 实际调用的这个构造方法。传入的int型初始化容量要>=0， 否则就是报不合法容量异常。


```java
public ArrayList(Collection<? extends E> c) {
        Object[] a = c.toArray();
        if ((size = a.length) != 0) {
            if (c.getClass() == ArrayList.class) {
                elementData = a;
            } else {
                elementData = Arrays.copyOf(a, size, Object[].class);
            }
        } else {
            // replace with empty array.
            elementData = EMPTY_ELEMENTDATA;
        }
    }
```

使用如 new ArrayList(otherList); 时候， 先把传入的集合转成了数组，size 被复制为传入集合的长度，如果是空集合，就用EMPTY_ELEMENTDATA来初始化，这是就相当于 new ArrayList();    如果非空集合，那么比较传入对象信息，如何是ArrayList 就直接把它元素复制给 elementData. 否则就是复制元素给elementData.
