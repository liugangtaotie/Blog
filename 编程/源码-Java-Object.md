## Object

我们知道，Java中所有类都继承了Object这个基类，那么我们今天来深入了解一下Object里面的各种方法。

```
private static native void registerNatives();
public final native Class<?> getClass();
public native int hashCode();
public boolean equals(Object obj){...}
protected native Object clone() throws CloneNotSupportedException;
public String toString(){...}
public final native void notify();
public final native void notifyAll();
public final native void wait(long timeout) throws InterruptedException;
public final void wait(long timeout, int nanos) throws InterruptedException{...}
public final void wait() throws InterruptedException{...}
protected void finalize() throws Throwable {}
```

## 重要方法详解

### registerNatives

静态块中调用，将Object基类中的native方法注册到本地

### getClass

返回当前运行时对象的Class对象

### hashCode

返回对象的哈希码，主要使用在哈希表中，默认是。

**哈希码定义**

1. 在java程序执行过程中，在一个对象没有被改变的前提下，无论这个对象被调用多少次，hashCode方法都会返回相同的整数值
2. 如果两个对象的equals方法为true，那么这两个对象的hashCode必须相同
3. 如果两个对象的hashCode方法一样，那么这两个对象的equals方法不一定为true

### equals

比较两个对象是否相等。默认实现，即比较2个对象的内存地址.

```
return (this == obj);
```

### toString



```
getClass().getName() + "@" + Integer.toHexString(hashCode())
```

## 参考

- https://fangjian0423.github.io/2016/03/12/java-Object-method/
- http://www.cnblogs.com/xdp-gacl/p/3636990.html
- https://gxnotes.com/article/35146.html

