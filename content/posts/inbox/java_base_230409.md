---
?title: "Java基础之集合"
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
## Java基础之集合总览 - ArrayList

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

### 常用的集合 - ArrayList

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

#### add

我们来看add方法：

```java

public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
}
```

添加元素时候，首先是  ensureCapacityInternal(size+1)  ，这个方法先不看，操作elementData，size+1 的位置放添加的元素e .  要保证成功放入元素，就要容量合适，所以ensureCapacityInternal 肯定会有容量操作的内容，带着思考去看ensureCapacityInternal.

```java
private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

private static int calculateCapacity(Object[] elementData, int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
}

private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
}
```

首先ensureCapacityInternal 的参数 minCapacity 就是新增元素后的最新长度，该方法只有一行实现： ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));

里面的方法是 calculateCapacity(elementData, minCapacity));  名字就是计算容量，它的具体实现就是首先判断出元素数组 elementData是否就是 DEFAULTCAPACITY_EMPTY_ELEMENTDATA, 如果是就返回 max(DEFAULT_CAPACITY, minCapacity);  DEFAULTCAPACITY_EMPTY_ELEMENTDATA 就是在 new ArrayList() 时候初始话的空数组实例。 DEFAULT_CAPACITY 就是 10 ， 所以if里面返回的就是 默认容量10 ，或者真实容量。 如果不是初始化0元素的数组，就直接返回真实长度。  总的来说，就是当第一次添加元素时候，走if里面的逻辑，返回默认容量10， 否则就返回真实容量。

再来看 ensureExplicitCapacity 方法， 首先 modCount++;  然后 如果 minCapacity - elementData.length >0，也就是 minCapacity > elementData.length ， 就调用 grow方法。也就是初始化时候是10， 超过10就增长 10 + 10/2 = 15 。

我们具体看grow方法：

```java
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
}
```

注意newCapacity = oldCapacity + (oldCapacity >> 1); 这里是新容量增长多少， 其实就是增加原来容量的一半。

如果 newCapacity < minCapacity 那么newCapacity就是 minCapacity

如果 newCapacity > MAX_ARRAY_SIZE ， 那么新容量就是 hugeCapacity(minCapacity), 这里MAX_ARRAY_SIZE 是

```java
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

```

也就是 2^31-1 - 8 .  最后调用 Arrays.copyOf 复制到新容量的数组。

其中如果 newCapacity > MAX_ARRAY_SIZE (2^31-1-8) , 那么最大容量会扩容：

```java
private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
}

```


当 minCapacity超过Integer.MAX_VALUE 时候会变成负数，这时就是内存溢出错误。


#### remove

```java
public E remove(int index) {
        rangeCheck(index);

        modCount++;
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }

```

remove方法， 首先是rankCheck, 检查要删的下表是否越界。需要移动的元素数量是  size - index -1; 然后复制到新数组，并把删除的位置index元素置为 null, 返回删除的元素。

我们主要看下移动了多少，System.arraycopy 。 上个 ``System.arraycopy(elementData, index+1, elementData, index,                              numMoved);`` 就是 本数组 index+1开始复制， 复制到本数组的  index 开始， numMoved 个元素。 最后一位是空的，就把它置为null.

```java
public static native void arraycopy(Object src,  int  srcPos,
                                        Object dest, int destPos,
                                        int length);
```


#### clear

```java
public void clear() {
        modCount++;

        // clear to let GC do its work
        for (int i = 0; i < size; i++)
            elementData[i] = null;

        size = 0;
    }
```

clear方法， for循环把所有的元素置为null, 然后size置为0
