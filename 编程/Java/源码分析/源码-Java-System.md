## System

System 类包含一些有用的类字段和方法。它不能被实例化。

在 System 类提供的设施中，有标准输入、标准输出和错误输出流；对外部定义的属性和环境变量的访问；加载文件和库的方法；还有快速复制数组的一部分的实用方法。


**用来初始化System的变量**

```
private static native void registerNatives();
```

**重新分配标准流（输入、输出、错误）**

```
public static void setIn(InputStream in)
public static void setOut(PrintStream out)
public static void setErr(PrintStream err)
```

**其他常见的一些方法**

```
public static Console console() // 返回系统控制台
public static Channel inheritedChannel() throws IOException // 返回从创建此 Java 虚拟机的实体中继承的信道。
public static SecurityManager getSecurityManager() // 则返回此安全管理器
public static native long currentTimeMillis(); // 当前时间
public static native long nanoTime();          // 系统计时器的当前值
// 从指定源数组中复制一个数组
public static native void arraycopy(Object src,  int  srcPos, Object dest, int destPos, int length)
public static native int identityHashCode(Object x); // 返回对象的哈希码，该代码与默认的方法 hashCode() 一样
public static Properties getProperties()             // 确定当前的系统属性
public static void setProperties(Properties props)   // 设定当前的系统属性
public static String getProperty(String key)
public static String getProperty(String key, String def)
public static String setProperty(String key, String value)
public static String clearProperty(String key)
public static String getenv(String name)            // 获取指定的环境变量值。环境变量是一个取决于系统的外部指定的值
public static java.util.Map<String,String> getenv() // 获取系统环境的字符串映射视图
public static void exit(int status)  // 系统退出
public static void gc()              // 运行垃圾回收器
public static void runFinalization() // 运行处于挂起终止状态的所有对象的终止方法
public static void runFinalizersOnExit(boolean value) // 已过时
public static void load(String filename)       // 加载动态库
public static void loadLibrary(String libname) // 加载动态库
public static native String mapLibraryName(String libname); // 将一个库名称映射到特定于平台的、表示本机库的字符串中
```

## 参考

- http://tool.oschina.net/apidocs/apidoc?api=jdk-zh
