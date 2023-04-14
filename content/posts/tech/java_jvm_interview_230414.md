---
title: "JVM 常见面试题"
date: 2023-04-14T04:21:08Z
draft: true
toc: false
images:
Series:
  - JVM
tags:
  - JVM
  - Java
  - 面试
---
## JVM 常见面试题

我让GPT生成了几个JVM常见面试题：

>
> 以下是一些常见的 JVM（Java 虚拟机）面试题：
>
> 1. 什么是 JVM？
> 2. JVM 的组成部分是什么？
> 3. 请解释 Java 内存模型（JMM）。
> 4. 什么是类加载器（Classloader）？请解释其工作原理。
> 5. 请列举 JVM 中的类加载器类型。
> 6. 什么是 Just-In-Time（JIT）编译器？它的作用是什么？
> 7. 请解释 Java 堆内存和栈内存的区别。
> 8. 什么是垃圾回收（Garbage Collection）？请简要描述其工作原理。
> 9. 请列举并简要介绍 Java 中的垃圾回收器。
> 10. 什么是分代垃圾回收策略？请解释年轻代（Young Generation）和老年代（Old Generation）的概念。
> 11. 请解释 Java 中的强引用、软引用、弱引用和虚引用。
> 12. 什么是 Java 堆溢出（Heap Overflow）和栈溢出（Stack Overflow）？如何处理这些问题？
> 13. 什么是逃逸分析？它在 JVM 性能优化中有什么作用？
> 14. 如何监控和调优 JVM 的性能？
> 15. 请列举 JVM 调优的一些常用参数。
> 16. 请解释 Java 中的死锁、饥饿和活锁。
> 17. 什么是线程安全？如何在 Java 中确保线程安全？
> 18. 请解释 Java 中的同步和异步编程。
> 19. 什么是 volatile 关键字？它的作用是什么？
> 20. 请解释 Java 中的并发编程和多线程编程。



### 1. 什么是 JVM？

JVM 是 Java Virtual Machine 的缩写，也就是Java虚拟机。它是一个运行Java程序的跨平台容器。它的主要功能特点是：

1. 加载、执行Java字节码
2. 内存管理
3. 热点代码优化
4. 跨平台

等。

> GPT 回答：
>
> Q: 什么是 JVM？
>
> A:  JVM（Java虚拟机）是一个抽象的计算机，它为运行 Java 字节码提供了运行时环境。JVM 可以使 Java 程序具有平台无关性，这意味着 Java 代码可以在任何支持 JVM 的操作系统上运行，而不需要为特定平台重新编译。Java 代码首先被编译成平台无关的字节码（.class 文件），然后在运行时由 JVM 解释执行或通过即时编译器（JIT）编译成本地机器代码。
>
> JVM 的主要功能包括：
>
> 1. 加载：将字节码加载到内存中，这涉及到类加载器（Classloader）和类的链接。
> 2. 执行：解释或编译字节码，执行程序逻辑。
> 3. 内存管理：负责内存分配和垃圾回收，管理 Java 堆、方法区、栈内存等。
> 4. 提供运行时环境：支持 Java 程序的运行，包括线程管理、异常处理、同步机制等。
>
> JVM 的核心组件包括：
>
> 1. 类加载器（Classloader）：负责加载字节码到内存中。
> 2. 执行引擎（Execution Engine）：解释执行字节码或通过即时编译器（JIT）编译成本地机器代码。
> 3. 垃圾回收器（Garbage Collector）：自动管理内存，回收不再使用的对象。
> 4. Java 内存模型（Java Memory Model）：包括 Java 堆、方法区、栈内存、程序计数器等。
>
> 总之，JVM 是 Java 程序运行时环境的核心，它使 Java 代码具有平台无关性，并提供内存管理、类加载和执行等功能。

### 2. JVM 的组成部分是什么？

根据以上回答，我们知道 JVM核心组件包括：

1. 类加载器 (ClassLoader): 负责加载字节码到内存中。
2. 执行引擎 (Execution Engine): 解释执行字节码或通过即时编译器(JIT)编译成本地机器代码
3. 垃圾回收器(Garbage Collector): 自动管理内存，回收不再使用的对象。
4. Java 内存模型(Java Memeory Model): 包括Java堆、方法区、栈内存、程序计数器等。

问下GPT ：

> Q: JVM 的组成部分是什么？
>
> A: JVM（Java虚拟机）的主要组成部分包括以下几个方面：
>
> 1. 类加载器（Classloader）：负责将字节码（.class 文件）加载到内存中。类加载器主要有三种：引导类加载器（Bootstrap ClassLoader）、扩展类加载器（Extension ClassLoader）和应用类加载器（Application ClassLoader）。
> 2. 运行时数据区（Runtime Data Area）：这是 JVM 内存管理的关键部分，包括以下几个区域：
>    * Java堆（Heap）：存储对象实例和数组，堆内存是所有线程共享的。
>    * 方法区（Method Area）：存储类结构信息，如类的名称、方法、字段等。方法区也是线程共享的。
>    * JVM栈（JVM Stack）：存储局部变量、操作数栈、方法调用帧等。每个线程都有自己的 JVM 栈。
>    * 程序计数器（Program Counter Register）：存储当前线程正在执行的字节码指令的地址。每个线程都有自己的程序计数器。
>    * 本地方法栈（Native Method Stack）：存储本地方法（例如 JNI 调用的方法）的调用帧。每个线程都有自己的本地方法栈。
> 3. 执行引擎（Execution Engine）：负责解释执行字节码或通过即时编译器（JIT，Just-In-Time Compiler）将字节码编译成本地机器代码来提高程序运行效率。执行引擎还包括一个垃圾回收器（Garbage Collector），负责自动回收不再使用的对象，以避免内存泄漏。
> 4. 本地方法接口（Java Native Interface，JNI）：JNI 是一个框架，允许 Java 代码与本地方法（通常是用 C 或 C++ 编写的）进行交互。通过 JNI，Java 代码可以调用本地方法，而本地方法也可以调用 Java 代码。
> 5. 本地方法库（Native Method Libraries）：这些库包含了与平台相关的本地方法实现，这些方法可以通过 JNI 框架被 Java 代码调用。
>
> JVM 的这些组成部分共同协作，为 Java 程序提供了一个抽象的、跨平台的运行时环境。

### 3. 请解释 Java 内存模型（JMM)

Java 内存模型就是JVM在运行时如何在内存中存储使用数据、管理内存。

> Q: 请解释 Java 内存模型
>
> A:  Java 内存模型（Java Memory Model，JMM）是一个抽象的概念，用于描述 Java 程序在运行时如何在内存中存储数据、如何管理内存以及线程之间如何进行数据共享。JMM 主要关注以下几个方面：
>
> 1. 内存分区：Java 内存模型将 JVM 的内存划分为不同的区域，以满足不同类型数据的存储需求。主要的内存区域包括：
>    * Java 堆（Heap）：用于存储对象实例和数组。堆内存是所有线程共享的，由垃圾回收器自动管理。
>    * 方法区（Method Area）：用于存储类信息（类名、方法、字段等）、常量池、静态变量等。方法区也是线程共享的。
>    * JVM 栈（JVM Stack）：用于存储局部变量、操作数栈、方法调用帧等。每个线程都有自己的 JVM 栈，栈内存不是线程共享的。
>    * 程序计数器（Program Counter Register）：存储当前线程正在执行的字节码指令的地址。每个线程都有自己的程序计数器。
>    * 本地方法栈（Native Method Stack）：用于存储本地方法（如 JNI 调用的方法）的调用帧。每个线程都有自己的本地方法栈。
> 2. 线程之间的内存可见性：JMM 定义了一套规则，确保一个线程对共享变量的修改对其他线程是可见的。这涉及到内存屏障、缓存一致性和处理器优化等底层概念。
> 3. 原子性、有序性和一致性：JMM 定义了原子操作、指令重排序和内存屏障等概念，以确保 Java 程序在多线程环境下能够正确地执行。
> 4. happens-before 关系：JMM 定义了一套 happens-before 规则，用于描述 Java 程序中的先行发生关系。这些规则有助于开发人员理解并发程序中的内存可见性和顺序一致性问题。
>
> 总之，Java 内存模型是一个抽象的概念，用于描述 Java 程序在运行时如何在内存中存储数据、如何管理内存以及线程之间如何进行数据共享。了解 Java 内存模型有助于编写正确的并发程序，并避免内存相关的问题。

这里有一个问题，方法区会存类的信息，静态变量， 那么静态方法会存在哪里？

答案是，方法区。 静态方法相当于类的一部分，属于类的信息。

### 4. 什么是类加载器（Classloader）？请解释其工作原理。

类加载器 Classloader 是JVM用来处理class文件的工具。主要工作就是把class 文件加载到内存并和JVM交互，用完卸载。

> Q: 什么是类加载器（Classloader）？请解释其工作原理。
>
> A: 类加载器（Classloader）是 Java 虚拟机（JVM）的一个组成部分，负责将编译后的字节码（.class 文件）加载到内存中。类加载器在运行时将字节码加载到 JVM 中，创建 Class 对象，然后这些 Class 对象可以被实例化为具体的对象。
>
> 类加载器的工作原理可以分为以下几个阶段：
>
> 1. 加载（Loading）：类加载器从文件系统、网络资源或其他来源读取字节码（.class 文件），将其加载到内存中，并创建一个对应的 Class 对象。Java 中有三种主要的类加载器：引导类加载器（Bootstrap ClassLoader）、扩展类加载器（Extension ClassLoader）和应用类加载器（Application ClassLoader）。
> 2. 链接（Linking）：链接过程将加载到内存中的字节码与 JVM 进行整合。链接过程包括以下三个子阶段：
>    * 验证（Verification）：确保加载的字节码是一个有效的、格式正确的 Java 类，没有安全问题。
>    * 准备（Preparation）：为类中的静态变量分配内存，并为其设置默认值。
>    * 解析（Resolution）：将常量池中的符号引用替换为直接引用，这涉及到解析类、接口、字段和方法等引用。
> 3. 初始化（Initialization）：执行类的静态初始化代码，包括静态变量的初始化和静态代码块的执行。这一过程保证了类在实例化之前已经完成了初始化。
> 4. 使用（Using）：在完成加载、链接和初始化后，类就可以被实例化并在程序中使用了。
> 5. 卸载（Unloading）：当类不再被使用时，类加载器可以将其从内存中卸载，以释放资源。
>
> 类加载器具有双亲委派模型（Parent Delegation Model）的特性，即当加载一个类时，类加载器会首先委托其父类加载器加载，如果父类加载器无法找到类，才会尝试自己加载。这种模型可以确保核心 Java 类库的一致性，避免类的重复加载。
>
> 总之，类加载器负责将字节码加载到内存中，使其在 JVM 中可用。类加载器的工作原理包括加载、链接、初始化、使用和卸载这几个阶段。



### 5. 请列举 JVM 中的类加载器类型

类加载有三种类型：

1. BootstrapClassLoader: 负责加载jdk的相关class文件
2. ExtensionClassLoader：负责加载jdk ext包下的class文件
3. ApplicationClassLoader: 负责加载应用程序的class文件

> Q: 请列举 JVM 中的类加载器类型
>
> A: 在 JVM 中，类加载器主要分为以下几种类型：
>
> 1. 引导类加载器（Bootstrap ClassLoader）：这是 Java 类加载器层次结构中的最顶层类加载器。它负责加载 Java 核心类库（如 java.lang, java.util 等），这些类库位于 JDK 安装目录下的 `jre/lib` 目录中。引导类加载器是用 C++ 语言实现的，因此在 Java 中无法获取它的引用。
> 2. 扩展类加载器（Extension ClassLoader）：扩展类加载器是引导类加载器的子类加载器，负责加载 Java 的扩展类库，如 `javax` 等。这些类库位于 JDK 安装目录下的 `jre/lib/ext` 目录中。扩展类加载器在 Java 中以 `java.net.URLClassLoader` 类的实例形式存在。
> 3. 应用类加载器（Application ClassLoader）：应用类加载器是扩展类加载器的子类加载器，也是 Java 应用程序的默认类加载器。它负责加载应用程序的类路径（classpath）上的类。应用类加载器也是以 `java.net.URLClassLoader` 类的实例形式存在。
>
> 除了这三种内置的类加载器外，Java 还支持自定义类加载器。开发人员可以通过继承 `java.lang.ClassLoader` 类并重写 `loadClass` 方法或其他相关方法来创建自己的类加载器。自定义类加载器可以用于实现热部署、隔离不同模块的类加载等高级功能。
>
> 总之，在 JVM 中，类加载器主要包括引导类加载器、扩展类加载器、应用类加载器以及自定义类加载器。

### 6. 什么是 Just-In-Time（JIT）编译器？它的作用是什么？

JIT编译器就是运行时解释器和优化器。我们知道Java源代码 .java 文件会编译成 .class 字节码文件，这些字节码文件被JVM的JIT解释执行，如果只是解释执行就相当于解释性语言了，如python.  JIT可以优化代码，把频繁执行的翻译成本地机器代码，直接调用执行。如果所有的class都编译成本地机器代码，那么这时候引入了AOT （Ahead Of Time） 的概念。

> Q: 什么是 Just-In-Time（JIT）编译器？它的作用是什么？
>
> A: Just-In-Time（JIT）编译器是 Java 虚拟机（JVM）的一个组成部分，用于在运行时将字节码（Java 中的中间代码形式）动态地编译成本地机器代码。JIT 编译器的主要目标是提高 Java 程序的执行性能。
>
> Java 程序在运行前需要经过两个阶段的编译过程。首先，Java 源代码被编译成字节码文件（.class 文件），然后这些字节码在 JVM 中被解释执行。解释执行字节码的过程相对较慢，因为每条字节码指令都需要在运行时转换为相应的机器指令。JIT 编译器的出现正是为了解决这个性能瓶颈。
>
> JIT 编译器的作用如下：
>
> 1. 提高性能：JIT 编译器将频繁执行的字节码（热点代码）编译成本地机器代码，使其能够直接在硬件上执行，从而大大提高程序的运行速度。
> 2. 优化代码：JIT 编译器在将字节码编译为本地代码时，可以应用各种优化技术，例如内联、循环展开、死代码消除等。这些优化进一步提高了生成的本地代码的执行效率。
> 3. 动态适应：由于 JIT 编译器是在运行时执行的，因此它可以根据实际运行情况（如执行频率、调用关系等）对代码进行优化。这种动态适应性使 JIT 编译器能够生成针对特定应用程序的高效本地代码。
>
> 总之，Just-In-Time（JIT）编译器是 JVM 的一个组成部分，用于在运行时将字节码动态地编译成本地机器代码。JIT 编译器通过提高性能、优化代码和动态适应来提高 Java 程序的执行效率。
>

补充学习：AOT Ahead Of Time, 运行之前就把class文件全编译成本地机器码行吗， 为何Java没这样做，我们来学习下：

> Q: JIT和AOT的比较
>
> A:  JIT（Just-In-Time）编译和 AOT（Ahead-Of-Time）编译都是将字节码编译成本地机器代码的技术，但它们在编译时机和策略方面有所不同。下面是 JIT 和 AOT 编译的比较：
>
> 1. 编译时机：
>    * JIT 编译：JIT 编译器在程序运行时对字节码进行动态编译。当程序运行时，JIT 编译器会监控哪些代码片段被频繁执行（热点代码），然后将这些代码编译成本地机器代码。JIT 编译器还可以根据程序的运行情况进行优化。
>    * AOT 编译：AOT 编译器在程序运行之前将字节码静态地编译成本地机器代码。这意味着在程序开始执行之前，所有的字节码都已经被转换为本地代码。因此，AOT 编译器无法根据程序的实际运行情况进行优化。
> 2. 优化策略：
>    * JIT 编译：由于 JIT 编译器在运行时工作，它可以根据程序的实际运行情况进行优化。这使得 JIT 编译器能够应用动态优化技术，例如内联缓存、动态反馈等。
>    * AOT 编译：AOT 编译器在程序运行之前完成编译，因此它无法利用运行时信息来进行优化。AOT 编译器主要依赖静态分析和启发式方法来进行优化。
> 3. 启动时间和运行时性能：
>    * JIT 编译：JIT 编译可能会导致程序的启动时间较长，因为在运行时需要对字节码进行编译。然而，由于 JIT 编译器可以针对程序的实际运行情况进行优化，所以它通常能够实现较高的运行时性能。
>    * AOT 编译：AOT 编译可以减少程序的启动时间，因为在运行时不需要对字节码进行编译。然而，由于 AOT 编译器无法利用运行时信息来进行优化，所以它可能无法实现与 JIT 编译器相同的运行时性能。
> 4. 适用场景：
>    * JIT 编译：适用于长时间运行的、对性能要求较高的应用程序，如服务器端应用、大型企业级应用等。
>    * AOT 编译：适用于对启动时间有严格要求的应用程序，如桌面应用、移动应用等。
>
> 总之，JIT 和 AOT 编译在编译时机、优化策略、启动时间和运行时性能等方面有所不同，它们各自具有优劣。在选择使用 JIT 还是 AOT 编译时，需要根据具体应用场景进行权衡。
>
> 对于需要长时间运行、对性能要求较高的应用程序，如服务器端应用、大型企业级应用等，JIT 编译器通常是更好的选择，因为它可以针对程序的实际运行情况进行优化，从而实现更高的运行时性能。
>
> 而对于对启动时间有严格要求的应用程序，如桌面应用、移动应用等，AOT 编译器通常是更好的选择，因为它可以在程序开始运行之前将字节码编译成本地机器代码，从而减少程序的启动时间。
>
> 需要注意的是，JIT 和 AOT 编译并不是互相排斥的关系，它们也可以一起使用。例如，Android 应用可以使用 AOT 编译器将 Java 代码编译成本地代码，同时使用 JIT 编译器对本地代码进行优化。这种混合使用的方式可以充分利用 JIT 和 AOT 编译器各自的优点，从而实现更好的性能和启动时间。

### 7. 请解释 Java 堆内存和栈内存的区别。

Java堆内存是Java GC管理的一块内存，主要存放运行时的方法之类，所有线程共享。栈内存是每个线程独有的，主要存放临时变量等。

> Q: 请解释 Java 堆内存和栈内存的区别
>
> A: Java 内存分为堆内存和栈内存两部分，它们在存储和管理对象时有着不同的作用和特点。
>
> 1. 堆内存（Heap Memory）：
>    堆内存用于存储 Java 中的对象实例，每个对象实例都分配在堆内存中。堆内存是一个共享的内存区域，被所有线程所共享。堆内存的大小可以通过启动参数 `-Xms` 和 `-Xmx` 进行设置。堆内存主要用于存储 Java 中的动态分配对象，例如数组和对象实例。在 Java 中，所有的类实例都是在堆上分配的。由于堆内存的共享性，它容易发生竞争和碎片问题，因此需要进行垃圾回收来维护堆内存的使用。
> 2. 栈内存（Stack Memory）：
>    栈内存用于存储 Java 中的局部变量和方法调用的状态。每个线程都拥有自己的栈内存，用于存储线程私有的数据。栈内存的大小由操作系统或 JVM 决定，并且在运行时是固定的。栈内存主要用于存储基本类型的变量和对象的引用类型变量。在 Java 中，所有的基本类型和对象引用变量都是在栈上分配的。当方法被调用时，栈帧被压入栈内存，方法执行完毕后，栈帧被弹出，栈内存的空间就被释放了。
>
> 总体来说，堆内存和栈内存在内存分配和释放、空间大小和生命周期等方面有所不同。堆内存用于存储动态分配的对象实例，大小可调整，生命周期长；栈内存用于存储局部变量和方法调用的状态，大小固定，生命周期短。在实际开发中，合理地使用堆内存和栈内存，可以提高程序的性能和稳定性。
>