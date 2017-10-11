
## 概要

```
java [ options ] class [ argument ... ]
java [ options ] -jar file.jar [ argument ... ]
javaw [ options ] class [ argument ... ]
javaw [ options ] -jar file.jar [ argument ... ]
说明
options:命令行选项
class:被调用的类名
file.jar:被调用的jar文件名，必须使用-jar选项.
argument:传递给main方法的参数
```

**JVM 参数分为三种**

- 标准参数：`-`，JVM基本参数，向后兼容；
- 非标准参数：`-X`，JVM底层配置参数，向后兼容；
- 非稳定参数：`-XX`，这类参数在JVM中是不稳定的，需要慎重使用；

## 标准参数

**-client**

设置jvm使用client模式，启动速度快，但性能和内存管理效率并不高，适用于客户端。

**-server**

设置jvm使用server模式，启动速度慢，但是性能和内存管理效率很高，适用于服务器。如果没有指定-server或-client，JVM启动的时候会自动检测当前主机是否为服务器(64位的JVM只有server模式，所以无法使用-client参数)。

**-DprotertyName=value**

定义系统全局属性值，如果value有空格，可以用`-Dname="space string"`这样的形式来定义，用`System.getProperty("propertyName")`获取value值

**-agentlib:libname[=options]**

加载载本地lib包，其中libname为本地代理库文件名，默认搜索路径为环境变量PATH中的路径，options为传给本地库启动时的参数，多个参数之间用逗号分隔。在Windows下搜索libname.dll，在linux下搜索libname.so，搜索路径环境变量在不同系统上有所不同，比如Solaries上就默认搜索LD_LIBRARY_PATH。

**-agentpath:pathname[=options]**

按全路径装载本地库，不再搜索PATH中的路径，其他功能和-agentlib相同。

**-verbose:jni**

输出jni方法的调用情况

**更多请参考**:`java -help` 或者 [官方文档](http://docs.oracle.com/javase/1.5.0/docs/tooldocs/windows/java.html)

## 非标准参数（扩展参数）

|参数|说明|
|---|---|
|-Xms|设置JVM初始化堆内存大小|
|-Xmx|设置JVM最大的堆内存大小|
|-Xss|设置JVM栈内存大小|
|-Xmn|设置young generation的堆内存大小,一般设置为Xmx的1/3|
|-Xmixed|混合模式执行|
|-Xint|解释模式执行|
|-Xbootclasspath:|设置zip/jar资源或者类(.class文件)存放目录路径|
|-Xbootclasspath/a:|追加zip/jar资源或者类(.class文件)存放目录路径|
|-Xbootclasspath/p:|预先加载zip/jar资源或者类(.class文件)存放目录路径|
|-Xnoclassgc|关闭类垃圾回收功能|
|-Xincgc|开启类垃圾回收功能|
|-Xloggc:|记录垃圾回日志到一个文件|
|-Xbatch|关闭后台编译|
|-Xprof|输入CPU概要表数据|
|-Xfuture|执行严格的代码检查,预测可能出现的情况|
|-Xrs|通过JVM还原操作系统信号|
|-Xcheck:jni|对JNI函数执行检查|
|-Xshare:off|尽可能不去使用共享类的数据|
|-Xshare:auto|尽可能的使用共享类的数据|
|-Xshare:on|尽可能的使用共享类的数据,否则运行失败|

## 非稳定参数

虚拟机内有很多非稳定参数，以下列表均为常用参数。

- `-XX:+<option>` 开启 option 参数
- `-XX:-<option>` 关闭 option 参数
- `-XX:<option>=<value>` 设置 option 参数值为 value

### 内存管理

|参数|默认值|使用介绍|
|---|---|---|
|DisableExplicitGC|关闭|忽略来自System.gc()方法触发的垃圾收集|
|UseConcMarkSweepGC|关闭|使用ParNew+CMS+Serial Old进行内存回收|
|UseSerialGC|Client开启/其他关闭|使用Serial+Serial Old进行内存回收|
|UseParallelGC|Server开启/其他关闭|使用Parallel Scavenge+Serial Old进行内存回收|
|UseParallelOldGC|关闭|使用Parallel Scavenge+Parallel Old进行内存回收|
|MaxPermSize|64MB|永久代的最大值|
|MaxNewSize|64MB|新生代的最大值|
|ParallelGCThreads|CPU个数|设置并行GC时内存回收的线程数|

### 即使编译

|参数|默认值|使用介绍|
|---|---|---|
|CompileThreshold|Client默认1500;Server默认1000|触发方法即时编译的阈值|
|ReservedCodeCacheSize|32MB|即时编译代码缓存的最大值|

### 类型加载

|参数|默认值|使用介绍|
|---|---|---|
|UseSplitVerifier|开启|使用依赖StackMapTable信息的类型检查代替数据流分析，以加快字节码校验速度|
|FailOverToOldVerifier|开启|校验失败时，是否允许回到老的类型推导校验方式进行校验|
|RelaxAccessControlCheck|关闭|放松对类型访问性的限制|

### 多线程相关

|参数|默认值|使用介绍|
|---|---|---|
|UseSpinning|JDK1.6开/JDK1.5关|开启自旋锁以避免频繁挂起和唤醒|
|PreBlockSpin|默认为10|使用自旋锁时的自旋次数|
|UseThreadPriorities|开启|使用本地线程优先级|
|UseBiasedLocking|开启|是否使用偏向所|
|UseFastAccessorMethods|开启|当频繁使用反射执行某个方法时，生成字节码来加快反射的执行速度|

### 性能参数

|参数|默认值|使用介绍|
|---|---|---|
|UseLargePages|开启|使用大内存分页|
|LargePageSizeInBytes|4MB|使用指定大小的内存分页|
|StringCache|开启|是否使用字符串缓存|

### 调试参数

|参数|默认值|使用说明|
|---|---|---|
|PrintGCDetails|关闭|每次GC时打印详细信息|
|PrintGCTimeStamps|关闭|打印每次GC的时间戳|
|TraceClassLoadingPreorder|关闭|跟踪被引用到的所有类的加载信息|

## 参考

- 深入理解Java虚拟机

- [官方文档：标准参数和非标准参数](http://docs.oracle.com/javase/1.5.0/docs/tooldocs/windows/java.html)

- [官方文档：非稳定参数](http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html)