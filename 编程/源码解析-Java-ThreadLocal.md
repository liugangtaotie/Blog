## 定义

ThreadLocal是一个关于创建线程局部变量的类，也叫线程本地变量。

通常情况下，变量是可以被任一线程访问并修改的。ThreadLocal 为变量在每个线程中都创建了一个副本，那么每个线程可以访问自己内部的副本变量，这样使用 ThreadLocal 创建的变量只能被当前线程访问，其他线程则无法访问和修改。

## 源码分析

> This class provides thread-local variables.  These variables differ from their normal counterparts in that each thread that accesses one (via its {@code get} or {@code set} method) has its own, independently initialized copy of the variable.  {@code ThreadLocal} instances are typically private static fields in classes that wish to associate state with a thread (e.g., a user ID or Transaction ID).

翻译如下

> ThreadLocal类用来提供线程内部的局部变量。这种变量在多线程环境下访问(通过get或set方法访问)时能保证各个线程里的变量相对独立于其他线程内的变量。ThreadLocal 实例通常来说都是 private static 类型的，用于关联线程和线程的上下文

ThreadLocal 源码实现原理

```
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }

    private T setInitialValue() {
        T value = initialValue();
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
        return value;
    }

    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }

     public void remove() {
         ThreadLocalMap m = getMap(Thread.currentThread());
         if (m != null)
             m.remove(this);
     }

    ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }

    void createMap(Thread t, T firstValue) {
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }

    static ThreadLocalMap createInheritedMap(ThreadLocalMap parentMap) {
        return new ThreadLocalMap(parentMap);
    }
```

上述代码相当精简，主要是把这个变量放到了当前线程Thread的threadLocals变量中，这样就把这个变量跟当前的线程绑定在了一起。查看Thread的源码我们发现

```
ThreadLocal.ThreadLocalMap threadLocals = null;
```

那么ThreadLocalMap这个类又有什么作用呢？

```
    static class ThreadLocalMap {
        static class Entry extends WeakReference<ThreadLocal<?>> {
            /** The value associated with this ThreadLocal. */
            Object value;

            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }
    }
```

ThreadLocalMap是一个映射表，key是使用ThreadLocal的弱引用，value是真正需要存储的Object，即Entry本质上就是WeakReference<ThreadLocal>。

也就是说，每个Thread都有一个ThreadLocalMap的映射表的引用，这样每个Thread中可以存放多个ThreadLocal对象，但却只有一个ThreadLocalMap的映射表。

## 常见用法

ThreadLocal适用于资源共享但不需要维护状态的情况，也就是一个线程对资源的修改，不影响另一个线程的运行；

## 应用场景

### 常规用法

```
public class DemoForThreadLocal {
    private static ThreadLocal<Integer> value = new ThreadLocal<Integer>() {
        @Override
        protected Integer initialValue() {
            return 0;
        }
    };
    public static void main(String[] args) {
        for (int i = 0; i < 3; i++) {
            int finalI = i;
            new Thread() {
                @Override
                public void run() {
                    super.run();
                    for (int j = 0; j < 5; j++) {
                        value.set(j);
                        System.out.println("Thread" + finalI + ":" + value.get());
                    }
                }
            }.start();
        }
    }
}
```

### 数据库连接

```
    private static ThreadLocal<Connection> connectionHolder = new ThreadLocal<Connection>() {
        public Connection initialValue() {
            return DriverManager.getConnection(DB_URL);
        }
    };

    public static Connection getConnection() {
        return connectionHolder.get();
    }
```

### Android Looper

这样可以让每个Thread只关联一个Looper，Looper的源码如下

```
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();
private static void prepare(boolean quitAllowed) {
    if (sThreadLocal.get() != null) {
        throw new RuntimeException("Only one Looper may be created per thread");
    }
    sThreadLocal.set(new Looper(quitAllowed));
}
```
 
## 内存泄露

由于ThreadLocalMap的Key是ThreadLocal的弱引用，因此一旦Key被GC回收，但线程仍然存在，则ThreadLocalMap仍然存在，那么很容易造成内存泄露。为了解决这个问题，设计了如下代码。

ThreadLocal.java

```
     public void remove() {
         ThreadLocalMap m = getMap(Thread.currentThread());
         if (m != null)
             m.remove(this);
     }
```

ThreadLocalMap.java

```
        private Entry getEntry(ThreadLocal<?> key) {
            int i = key.threadLocalHashCode & (table.length - 1);
            Entry e = table[i];
            if (e != null && e.get() == key)
                return e;
            else
                return getEntryAfterMiss(key, i, e);
        }
        private Entry getEntryAfterMiss(ThreadLocal<?> key, int i, Entry e) {
            Entry[] tab = table;
            int len = tab.length;

            while (e != null) {
                ThreadLocal<?> k = e.get();
                if (k == key)
                    return e;
                if (k == null)
                    expungeStaleEntry(i);
                else
                    i = nextIndex(i, len);
                e = tab[i];
            }
            return null;
        }
```

## 参考资料

- 《深入理解Java虚拟机》
- 《Java编程思想》
- http://www.cnblogs.com/dolphin0520/p/3920407.html
- http://droidyue.com/blog/2016/03/13/learning-threadlocal-in-java/index.html
- http://blog.csdn.net/u010412719/article/details/52388978
- http://www.sczyh30.com/posts/Java/java-concurrent-threadlocal/
- http://blog.csdn.net/singwhatiwanna/article/details/48350919
- http://blog.xiaohansong.com/2016/08/06/ThreadLocal-memory-leak/