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

返回对象的哈希码，主要使用在哈希表中，默认实现是将对象在内存中地址转换为一个整数。

**哈希码定义**

1. 在java程序执行过程中，在一个对象没有被改变的前提下，无论这个对象被调用多少次，hashCode方法都会返回相同的整数值
2. 如果两个对象的equals方法为true，那么这两个对象的hashCode必须相同
3. 如果两个对象的hashCode方法一样，那么这两个对象的equals方法不一定为true

具体应用可参考HashMap源码

### equals

比较两个对象是否相等。默认实现，即比较2个对象的内存地址。

```
return (this == obj);
```

**特性**

1. 自反性：对于非空对象x,x.equals(x)为true
2. 对称性：x.equals(y) == y.equals(x)
3. 传递性：如果 x.equals(y) 且 y.equals(z) 则 x.equals(z) 
4. 一致性：x.equals(y) 的结果在x对象与y对象在equals比较中所用的信息没有修改的前提下，保持不变。

**注意**

如果重写了equals方法，那么通常有必要重写hasCode方法

### clone

创建并返回当前对象的一份拷贝，一般来说，属性完全一致，但内存地址不同。

```
// 结果为true
x.clone() != x
// 结果为true
x.clone().getClass() == x.getClass()
// 结果为true
x.clone().equals(x)
```

若想使用clone方法必须实现Cloneable接口，否则会报错java.lang.CloneNotSupportedException

- **浅拷贝** 使用一个已知实例对新创建实例的成员变量逐个赋值，这个方式被称为浅拷贝。（默认clone方法即浅拷贝）
- **深拷贝** 当一个类的拷贝构造方法，不仅要复制对象的所有非引用成员变量值，还要为引用类型的成员变量创建新的实例，并且初始化为形式参数实例值。这个方式称为深拷贝。（深拷贝可以自定义，也可以使用序列化实现）

```
    public static <T extends Serializable> T clone(T obj) {
        T cloneObj = null;
        try {
            ByteArrayOutputStream out = new ByteArrayOutputStream();
            ObjectOutputStream obs = new ObjectOutputStream(out);
            obs.writeObject(obj);
            obs.close();

            ByteArrayInputStream ios = new ByteArrayInputStream(out.toByteArray());
            ObjectInputStream ois = new ObjectInputStream(ios);
            cloneObj = (T) ois.readObject();
            ois.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return cloneObj;
    }
```

### toString

该类的对象以字符串风格显示输出，默认实现是类名@哈希码的十六进制

```
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

### 监视器

顾名思义，监听对象操作，主要是协调当前对象多线程之间的执行顺序。监视器是一种多线程的同步结构。

可以将监视器看做一个包含特殊对象的特殊房间，一次只能被一个线程占用。主要有以下几种操作

1. 进入监视器
2. 获取监视器
3. 拥有监视器
4. 释放监视器
5. 退出监视器

每个对象默认都绑定了一个监视器，具体有以下三种方法。

```
wait()      // 获取监视器
notify()    // 释放监视器
notifyAll() // 释放所有监视器
```

### 锁

实现监视器的具体方式，即协调多线程之间执行顺序、实现多线程同步的具体方式。相当于上一小节中进入监视器和退出监视器的具体实现。

加锁的方式有以下几种

1. 使用 synchronized （wait-notify）
2. 使用 Lock （信号量机制）
3. 静态同步方法

### wait

让当前线程T等待，并释放该对象的锁，该线程T不可用且处于休眠状态，直到发生下面四种情况之一

1. 其他某个线程调用此对象的notify方法，线程T碰巧被任选为被唤醒的线程
2. 其他某个线程调用此对象的notifyAll方法
3. 其他某个线程调用Thread.interrupt方法中断线程T
4. 时间超时（timeout如果为0的话，则会一直等待）

### notify

唤醒在此对象监视器上等待的线程，如果有多个线程等待，那么会随机任意选择一个线程。

**需要注意**

notify方法只能被拥有此对象监视器的所有者的线程来调用，一次只能有一个线程拥有该对象的监视器，一个对象加锁以后即变成为对象监视器的所有者，如果当前线程不是此对象监视器所有者调用的话会抛出IllegalMonitorStateException异常。

### notifyAll

唤醒此对象监视器上等待的所有线程。

### finalize

实例被垃圾回收器回收的时候所触发的操作，即将回收对象前的最后一个操作，可以理解为对象前的垂死挣扎。

## 参考

- https://fangjian0423.github.io/2016/03/12/java-Object-method/
- https://gxnotes.com/article/50682.html
- https://gxnotes.com/article/35146.html
- https://www.kancloud.cn/digest/java-thread/107456
- http://www.jianshu.com/p/3e06033b0326
- http://www.cnblogs.com/keeplearnning/p/7020287.html