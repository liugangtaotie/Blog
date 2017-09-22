## 内存模型

Java内存模型，往往是指Java程序运行在Java虚拟机时内存的模型。

![内存模型](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/jvm-01.png)

JVM运行时数据区的树形结构如下

```
私有数据区
    程序计数器
    虚拟机栈
    本地方法栈
共享数据区
    堆区
    方法区
        常量池
        其他
# 注意：上述为Java6，方法区及常量池在 Java7（在堆区）、Java8 中存放位置发生变化。
```

**程序计数器**

程序计数器是一块较小的「线程私有」的内存空间，可以看作当前线程所执行字节码的行号指示器。

在字节码解释器的工作时，就是通过改变程序计数器的值来读取下一条需要执行的字节码指令。比如：分支、循环、跳转、异常处理、线程恢复等

如果线程正在执行一个Java方法，则此计数器记录正在执行的虚拟机字节码指令地址；如果线程正在执行的是Native方法，则此计数器值为空（undefined）。此内存区域是唯一没有OOM情况的区域。

**虚拟机栈**

每个方法在执行的同时会创建一个*栈帧*，用来存储：局部变量表、操作数栈、动态链接、方法出口、等信息。

局部变量表是在编译期间完成内存分配，存放了各种基本数据类型和对象引用。

如果线程请求栈深度大于当前虚拟机所允许深度，会抛出StackOverflowError异常；如果虚拟机无法申请到足够的内存，会抛出OutOfMemoryError异常。

**本地方法栈**

虚拟机栈是为虚拟机执行Java方法服务，本地方法栈是为虚拟机Native方法服务。

同虚拟机栈一样，本地方法栈也可能会抛出StackOverflowError和OutOfMemoryError异常。

**Java堆**

> The heap is the runtime data area from which memory for all class instances and arrays is allocated

几乎所有的对象示例都在这里分配内存。（随着编译器的发展以及逃逸分析技术的成熟，所有的对象分配在堆上也不是那么「绝对」了）

**方法区**

存储已被虚拟机加载的类信息、常量、静态变量、即时编译后代码等数据。

虽然Java虚拟机规范将方法区描述为堆的一个逻辑部分，物理上它还是在堆上创建的，但是它的别名是`Non-Heap`，就是想跟Java堆区分开来。

**常量池**

是方法区的一部分，用于存放编译器时期生成的各种常量。常量池具备动态性，运行期间可以通过`str.intern()`动态存放常量到池中。常量池在 Java7、Java8 中存放位置发生变化。后面还有介绍。

**直接内存**

JDK 1.4 以后加入了NIO(New Input/Output)类，引入了基于通道的I/O方式，可以使用Native函数库分配堆外内存。

## 堆内存分布

堆内存分为三个部分，分别称为新生代、老年代和永久代。

新生代和老年代是虚拟机规范里面的堆区，永久代是虚拟机规范里面的方法区，是方法区的一种实现，但实际上方法区也是堆上创建。

![堆内存分布](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/jvm-02.png)

### 新生代(Young)

新生代又进一步分为Eden区、Survivor1区、Survivor2区。

新创建的对象会分配在Eden区（一些大对象、长字符串、数组可能会直接存放到老年代）,在经历一次GC后会被移到Survivor1区，再经历一次GC后会被移到Survivor2区，直到升至老年代。

### 老年代(Old)

主要存放应用程序中生命周期长的内存对象。

### 永久代(Permanent)

方法区的一种实现，是指内存的永久保存区域，永久代中用于存放类和方法的元数据以及常量池，在Java中对应能通过反射获取到的数据，比如Class和Method。每当一个类初次被加载的时候，它的元数据都会放到永久代中。

但是，永久代是有大小限制的，因此如果加载的类太多，很有可能导致永久代内存溢出 `java.lang.OutOfMemoryError: PermGen`。

为了解决这个问题，Java开始移除永久代，jdk1.6常量池放在方法区，jdk1.7常量池放在 Java Heap，jdk1.8常量池放在 Metaspace 。（**注意:** 字符串常量池移从jdk1.7以后移至Java Heap）

### 元空间(Metaspace)

> In JDK 8, classes metadata is now stored in the native heap and this space is called Metaspace.

这样就可以解决OOM问题：类的元数据分配只受本地内存大小的限制，可使用`-XX:MaxMetaspaceSize`参数来指定Metaspace的大小。

还有很多其他的优点如下

> 1.Take advantage of Java Language Specification property : Classes and associated metadata lifetimes match class loader’s
  2.Linear allocation only
  3.No individual reclamation (except for RedefineClasses and class loading failure)
  4.No GC scan or compaction
  5.No relocation for metaspace objects
  
## 常量池

这里单独讲解说明一下常量池，常量池存放编译器时期生成的各种常量。

常量池具备动态性，运行期间可以通过`str.intern()`动态存放常量到池中。

java中基本类型的包装类的大部分都实现了常量池技术。

**什么是常量**

final 修饰的静态变量、实例变量和局部变量都是常量

**什么是常量池**

Java中的常量池，实际分为两种：静态常量池和运行时常量池。

- 静态常量池，即class文件中的常量池，不仅包含类的版本、字段、方法、接口等描述信息，还有一项信息是常量池，用于存放编译期生成的各种常量，这部分内容将在类加载后进入方法区的运行时常量池中存放。

- 运行时常量池，即我们常说的方法区中的运行时常量池，是JVM在完成类装载操作后，将class文件中的常量池载入到内存中，并保存在方法区中。主要存放两大类常量：字面量（java层面的常量）和符号引用（包名、类名、字段名、方法名）。

**存放位置**

jdk1.6常量池放在方法区，jdk1.7常量池放在 Java Heap，jdk1.8常量池放在 Metaspace （**注意:** 字符串常量池移从jdk1.7以后移至Java Heap）

## 参考

- [探秘Metaspace](http://www.sczyh30.com/posts/Java/jvm-metaspace/)
- [Metaspace in Java 8](http://java-latte.blogspot.sg/2014/03/metaspace-in-java-8.html)
- [Remove the Permanent Generation](http://openjdk.java.net/jeps/122)
- [What is the use of MetaSpace in Java 8?](http://stackoverflow.com/questions/24074164/what-is-the-use-of-metaspace-in-java-8)
- [Java 8: 从永久代（PermGen）到元空间（Metaspace）](http://blog.csdn.net/zhyhang/article/details/17246223/)
