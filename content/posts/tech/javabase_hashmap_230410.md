---
title: "Java集合基础之HashMap"
date: 2023-04-10T08:24:53Z
draft: false
toc: false
images:
series:
  - Java基础
tags:
  - Java
  - 面试
---
## Java集合基础之HashMap

HashMap在日常中也是使用频繁，了解它的源码很有必要。

### 构造方法与默认属性

```
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {}
```

```
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
```

默认容量的16， ArrayList是10。

```
static final int MAXIMUM_CAPACITY = 1 << 30;
```

最大容量是 1<<30

```
static final float DEFAULT_LOAD_FACTOR = 0.75f;
```

默认负载因子是 0.75.

```
static final int TREEIFY_THRESHOLD = 8;
```

当>=8时候转化为红黑树

```
static final int UNTREEIFY_THRESHOLD = 6;
```

```
static final int MIN_TREEIFY_CAPACITY = 64;
```

核心内部类Node

```java
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }

        public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
    }
```

Map.Entry 类型的 Entry, K , V 键值对，用于存信息。重写hashCode 和 equals方法。 equals 主要逻辑是， 如果比较的是自身 (o == this), 直接返回true.  否则就要 key , value 都要相等。

```
transient Node<K,V>[] table;
```

Node类型数组，存hash表。

```
transient Set<Map.Entry<K,V>> entrySet;
```

entrySet

先来看无参数构造：

```java
public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
}
```

所有属性都是默认。(默认长度2<<4 是16， 负载因子、列表转数等)

```java
    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }

    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }
```

传入初始化容量 initialCapacity ,  首先不能小于0， 不能大于最大容量，这里最大容量是：

```
static final int MAXIMUM_CAPACITY = 1 << 30;
```

不同于 ArrayList的  2^31 - 1 - 8  。

看最后的

```java
this.threshold = tableSizeFor(initialCapacity);

 /**
   * Returns a power of two size for the given target capacity.
   * 返回给定目标容量的两个大小的幂
   */
static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```

这个threshold

```java
/**
     * The next size value at which to resize (capacity * load factor).
     *
     * @serial
     */
    // (The javadoc description is true upon serialization.
    // Additionally, if the table array has not been allocated, this
    // field holds the initial array capacity, or zero signifying
    // DEFAULT_INITIAL_CAPACITY.)
    int threshold;
```








### 常用方法
