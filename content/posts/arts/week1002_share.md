---
title: "Week1002_share"
date: 2023-03-31T15:45:31Z
draft: false 
toc: false
images:
categorys:
  - 每周学习
series:
tags:
  - arts 
---

## Java 内存区域及对象
1. 计算机存储单位
从小到大 Bit byte KB MB GB TB 
2. 计算机存储元件
	+ 寄存器 CPU读写数据存储元件
	+  内核空间与用户空间
	+  字长 CPU一次能并行处理二进制的位数
3. 堆栈
	堆用来存储对象，栈用来执行程序
	
4. 运行时数据区域


	![JVM内存区域](http://images.blogjava.net/blogjava_net/nkjava/jvmstructure.png)
	
	1. 线程独有的内存区域
		a. Program counter register 程序计数器
		当前线程执行的字节码行号指示器
		b. Java Stack 虚拟机栈
		生命周期和线程相同。每个方法执行时候都会创建一个栈帧，用于存储局部变量表，操作数栈，动态链接、方法出口等信息，每个方法从调用到执行的过程，就对应着一个栈帧在虚拟机中入栈到出栈的过程。栈的大小通常与虚拟机实现有关，通常在256K-756K之间。
		c. Native Method stack  本地方法栈
		和虚拟机栈作用一样，方法栈为虚拟机使用的本地方法服务。
	2. 线程间共享的内存区域
		a. heap ,堆
		存放对象实例。细分为新生代和老年代。
		b. Method Area 方法区
		存储虚拟机加载的类信息，常亮、静态变量、即时编译器编译后的代码等数据。
		c. Runtime Constant Pool 运行时常量池
		类的方法、字段、接口库等描述信息 + 编译期产生的各种字面量和符号引用
		
	3. 直接内存

### 对象创建
语言层面，new, 虚拟机层面

1. 虚拟机遇到new指令，首先去检查这个指令的参数能否在常亮池中定位到一个类的符号引用，并且检查这个符号引用代表的类是否已经被加载、解析和初始化。如果没有就进行类的初始化过程。
2. 类加载检查通过后，为新生对象分配内存，过程中，
	
	+ a. 若内存规整，指针碰撞法分配内存。 意思是所有已用内存在一边，空闲在另一边，中间放着一个指针作为分界点的指示器，分配内存就仅仅是把指针向空闲那边挪动一段与对象大小相等的距离罢了。如果垃圾收集器选择的是Serial、ParNew这种基于压缩算法的，虚拟机采用这种分配方式
	+ b. 如果内存不规整，虚拟机使用空闲列表法来分配内存。意思是虚拟机维护了一个列表，记录上哪些内存块是可用的，再分配的时候从列表中找到一块足够大的空间划分给对象实例，并更新列表上的内容。如果垃圾收集器选择的是CMS这种基于标记-清除算法的，虚拟机采用这种分配方式。
	
	另外一个问题及时保证new对象时候的线程安全性。因为可能出现虚拟机正在给对象A分配内存，指针还没有来得及修改，对象B又同时使用了原来的指针来分配内存的情况。虚拟机采用了CAS配上失败重试的方式保证更新更新操作的原子性和TLAB两种方式来解决这个问题。
		
3. 内存分配结束，虚拟机将分配到的内存空间都初始化为零值（不包括对象头）。这一步保证了对象的实例字段在Java代码中可以不用赋初始值就可以直接使用，程序能访问到这些字段的数据类型所对应的零值。
4. 对对象进行必要的设置，例如这个对象是哪个类的实例、如何才能找到类的元数据信息、对象的哈希码、对象的GC分代年龄等信息，这些信息存放在对象的对象头中。
5. 执行init方法，把对象按照程序员的意愿进行初始化，这样一个真正可用的对象才算完全产生出来。


### 对象定位方式
建立对象是为了使用对象，Java程序需要通过栈上reference数据来操作堆上具体对象。

```
Object obj = new Object();
```

new Object() 后有两部分内容，一部分是类数据（比如代表类的Class对象）、一部分是实例数据

由于reference 在Java虚拟机规范中这是一个指向对象的new Object() 的引用obj,并没有规定obj
应该通过何种方式去定位、访问堆中对象的具体位置，所以对象访问方式是取决于虚拟机实现而定的。主流方式有两种：

1. 句柄访问。Java堆中划分出一块句柄池，obj指向的是对象的句柄地址，句柄则包含了类数据的地址和实例数据的地址
2. 指针访问。对象中存储所有的实例数据和类数据的地址，obj指向的是这个对象

Hotspot 虚拟机采用的后者。
			