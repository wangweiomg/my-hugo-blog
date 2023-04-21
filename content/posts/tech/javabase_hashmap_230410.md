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

#### put

```java
public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
	// Node 数组 tab;   Node 对象p;  整型  n , i;
        Node<K,V>[] tab; Node<K,V> p; int n, i;

	// table赋值给 tab ，这里table 是成员变量 transient Node<K,V>[] table;
	// 如果table 是null 或者 tab的长度是0， 就resize() ， 赋值给tab 和 n, resize的具体实现先不看，以免影响主线，根据名字，就当做是调整初始化size的方法
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
	// 这时候就要找到桶的位置， 算法就是 (n-1) & hash  ？？？ 
	// 如果值为空，也就是桶的位置没有值， 就新建一个node 放进去
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else { // 否则，桶的位置不为空，就遍历链表或红黑树
            Node<K,V> e; K k;
	    // 如果hash 相同 并且 (p.key == key 或者 key 不为null p.key.equals(key)))
	    // 如果hash 相同并且 key相同或值相等 ，就把 p 赋值 给e
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
	    // 如果 p 是一个TreeNode结构，就去遍历红黑树，返回e
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
		// 如果取出的第一个元素不一样，就遍历链表
                for (int binCount = 0; ; ++binCount) {
		    // 如果next 是null， 就拼接上新node
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                       // 如果 binCount >= 8-1 ,就转化为 treeifyBin
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    // next不为空，hash相等就用 equals比较key
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
   	    // 存在key映射， 就把value赋值给e的value, 然后调用 afterNodeAccess(e)
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

首先来看最常用的put方法。 它接受一个 key 和 value. 我们知道hashmap会把key计算hash 还会处理hash碰撞， 重点带着问题去看代码。

put方法调用了 putVal() 方法，   hash(key), key, value, false, true

这几个参数分别是

```
/**
 * Implements Map.put and related methods.
 *
 * @param hash hash for key   key的hash值
 * @param key the key   key的值
 * @param value the value to put  value的值
 * @param onlyIfAbsent if true, don't change existing value  如果true,不改变存在的值
 * @param evict if false, the table is in creation mode. 如果false, table就是创建模式
 * @return previous value, or null if none
 */
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {}
```

##### 如何获取到桶的位置？

```java
if ((p = tab[i = (n - 1) & hash]) == null)
```

看这里这一段， if ((p = tab[i = (n - 1) & hash]) == null)，  获得桶的位置，也就是要存的数组的下表 i 的计算方式：

> i = (n-1) & hash

这里的n 来自上一步, n 是 tab.length ， 如果是tab是空的， n 来自resize()后 tab的length , 反正n 就是map中元素数组的长度.

```java
if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
```

再来看hash是哪个， hash 来自于传入put的key 的hash值， 也就是hash = hash(key) , hash算法是:

```java
/**
     * Computes key.hashCode() and spreads (XORs) higher bits of hash
     * to lower.  Because the table uses power-of-two masking, sets of
     * hashes that vary only in bits above the current mask will
     * always collide. (Among known examples are sets of Float keys
     * holding consecutive whole numbers in small tables.)  So we
     * apply a transform that spreads the impact of higher bits
     * downward. There is a tradeoff between speed, utility, and
     * quality of bit-spreading. Because many common sets of hashes
     * are already reasonably distributed (so don't benefit from
     * spreading), and because we use trees to handle large sets of
     * collisions in bins, we just XOR some shifted bits in the
     * cheapest possible way to reduce systematic lossage, as well as
     * to incorporate impact of the highest bits that would otherwise
     * never be used in index calculations because of table bounds.
     */
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

先看算法， 如果key 是null, hash(null) 就是0 ， 否则就是 (h = key.hashCode())^(h>>>16)

也就是 key.hashCode() ^ (key.hashCode()>>>16)

连到上面计算 ``i = (n-1) & hash`` 的式子里

i = (tab.length-1) & (key.hashCode() ^ (key.hashCode() >>>16))

这个到底是什么意思呢? 如果我来设计，我一般能想到计算桶的位置，获取到hash值后直接和长度取模运算，也就是 hash%tab.length ，避免占到0位置的null ，就要写成  hash%(tab.lenght-1) + 1， 这个也基本是java5的思想。

Java5计算桶位置的算法：

```java
int hash = hash(key.hashCode());
int i = (hash & 0x7FFFFFFF) % table.length;

```

对key的hashcode再hash, 然后和最大int值按位与，保证是正整数， 然后在取模table.length, 这样结果就是 0 - lenght-1 之间。

由于负载因子增多时候，扩容需要重新计算所有hash ，所以会出现性能问题，hash碰撞概率也会变大。因此在java6中改造为：

```java
static int hash(int h) {
    // This function ensures that hashCodes that differ only by
    // constant multiples at each bit position have a bounded
    // number of collisions (approximately 8 at default load factor).
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);
}
/**
 * Returns index for hash code h.
 */
static int indexFor(int h, int length) {
    return h & (length-1);
}
```

`(n - 1) & hash`，其中 `n` 表示 HashMap 的容量，`hash` 表示键的哈希值。 这个算法虽然采用了位运算的方式计算桶位置，具有较高的计算效率，但是在负载因子较高时会出现性能问题和哈希冲突的概率变高等问题。

所以Java7又改版了, 修改了hash计算方式：

```java
final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}

/**
 * Returns index for hash code h.
 */
static int indexFor(int h, int length) {
    return h & (length-1);
}
```

然后就是java8中的桶计算方式了：

```java
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

```

```java
i = (n-1) & hash
```

和java7的 indexFor 一样， 不同的是，在hash碰撞时候，链表超过7会转化为红黑树，加速查询。

之后的版本，hashmap的桶计算方法，就没有大改过了。

##### 如何转为红黑树的

在putVal方法中，当hash碰撞发生, 桶的位置会用链表存元素，当元素>=8 个时候，就会调用 treeifyBin(tab, hash)

```java
if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
          treeifyBin(tab, hash);
```

再来看看 treeibyBin方法， do-while循环主要就是把 链表的元素转换成 TreeNode 形式，之后调用treeify方法完成转换

```java
final void treeifyBin(Node<K,V>[] tab, int hash) {
        int n, index; Node<K,V> e;
        if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
            resize();
        else if ((e = tab[index = (n - 1) & hash]) != null) {
            TreeNode<K,V> hd = null, tl = null;
            do {
                TreeNode<K,V> p = replacementTreeNode(e, null);
                if (tl == null)
                    hd = p;
                else {
                    p.prev = tl;
                    tl.next = p;
                }
                tl = p;
            } while ((e = e.next) != null);
            if ((tab[index] = hd) != null)
                hd.treeify(tab);
        }
    }
```

再来看看treeify方法:

```java
final void treeify(Node<K,V>[] tab) {
            TreeNode<K,V> root = null;
            for (TreeNode<K,V> x = this, next; x != null; x = next) {
                next = (TreeNode<K,V>)x.next;
                x.left = x.right = null;
                if (root == null) {
                    x.parent = null;
                    x.red = false;
                    root = x;
                }
                else {
                    K k = x.key;
                    int h = x.hash;
                    Class<?> kc = null;
                    for (TreeNode<K,V> p = root;;) {
                        int dir, ph;
                        K pk = p.key;
                        if ((ph = p.hash) > h)
                            dir = -1;
                        else if (ph < h)
                            dir = 1;
                        else if ((kc == null &&
                                  (kc = comparableClassFor(k)) == null) ||
                                 (dir = compareComparables(kc, k, pk)) == 0)
                            dir = tieBreakOrder(k, pk);

                        TreeNode<K,V> xp = p;
                        if ((p = (dir <= 0) ? p.left : p.right) == null) {
                            x.parent = xp;
                            if (dir <= 0)
                                xp.left = x;
                            else
                                xp.right = x;
                            root = balanceInsertion(root, x);
                            break;
                        }
                    }
                }
            }
            moveRootToFront(tab, root);
        }
```

#### remove

```java
public V remove(Object key) {
        Node<K,V> e;
        return (e = removeNode(hash(key), key, null, false, true)) == null ?
            null : e.value;
    }

final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
        Node<K,V>[] tab; Node<K,V> p; int n, index;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (p = tab[index = (n - 1) & hash]) != null) {
            Node<K,V> node = null, e; K k; V v;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                node = p;
            else if ((e = p.next) != null) {
                if (p instanceof TreeNode)
                    node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
                else {
                    do {
                        if (e.hash == hash &&
                            ((k = e.key) == key ||
                             (key != null && key.equals(k)))) {
                            node = e;
                            break;
                        }
                        p = e;
                    } while ((e = e.next) != null);
                }
            }
            if (node != null && (!matchValue || (v = node.value) == value ||
                                 (value != null && value.equals(v)))) {
                if (node instanceof TreeNode)
                    ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
                else if (node == p)
                    tab[index] = node.next;
                else
                    p.next = node.next;
                ++modCount;
                --size;
                afterNodeRemoval(node);
                return node;
            }
        }
        return null;
    }
```

remove方法为删除一个hashmap节点
