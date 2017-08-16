
## Reference

> Abstract base class for reference objects.  This class defines the operations common to all reference objects.  Because reference objects are implemented in close cooperation with the garbage collector, this class may not be subclassed directly.

## 强引用 FinalReference

程序中有直接可达的引用，而不需要通过任何引用对象，

```
Object obj = new Object();
```

## 软引用 SoftReference

软引用的对象，只有在内存不足的时候（抛出OOM异常前），垃圾收集器会决定回收该软引用所指向的对象。软引用通常用于实现内存敏感的缓存。

```
SoftReference<Object> softRef = new SoftReference<Object>(new Object());
```

## 弱引用 WeakReference

弱引用的对象，不管内存是否足够，只要被垃圾收集器发现，只要GC的时候，该引用的对象就会被回收（具体可参见 WeakHashMap 源码中的 Entry 类）。

```
WeakReference<Object> weakRef = new WeakReference<Object>(new Object());
```

## 虚引用 PhantomReference

虚引用，该引用必须和引用队列(ReferenceQueue)一起使用，它通常用来在一个对象被GC前作为一个GC的标志，以此来做一些finalize操作。一般用于实现追踪垃圾收集器的回收动作，比如在对象被回收的时候，会调用该对象的finalize方法，在使用虚引用可以实现该动作，也更加安全。

```
Object obj = new Object();
ReferenceQueue<Object> refQueue = new ReferenceQueue<>();
PhantomReference<Object> phantom = new PhantomReference<Object>(obj, refQueue);
```

## 引用队列 ReferenceQueue

> Reference queues, to which registered reference objects are appended by the garbage collector after the appropriate reachability changes are detected.

翻译：在适当的时候检测到对象的可达性发生改变后，垃圾回收器就将已注册的引用对象添加到此队列中。

创建Reference时，将Queue注册到Reference中，当该Reference所引用的对象被垃圾收集器回收时，会将该Reference放到该队列中，类似于一种通知机制。

```
ReferenceQueue queue = new ReferenceQueue();
WeakReference reference = new WeakReference(new Object(), queue);
System.out.println(reference);
System.gc();
Reference reference1 = queue.remove();
System.out.println(reference1);
```

## ReferenceHandler

> High-priority thread to enqueue pending References

用于实现将pending队列里面的Reference实例依次添加到不同的ReferenceQueue中（取决于Reference里面的queue）。该pending的元素由GC负责加入。

## Reference 四种状态

> Active: Subject to special treatment by the garbage collector.  Some time after the collector detects that the reachability of the referent has changed to the appropriate state, it changes the instance's state to either Pending or Inactive, depending upon whether or not the instance was registered with a queue when it was created.  In the former case it also adds the instance to the pending-Reference list.  Newly-created instances are Active.

> Active: queue = ReferenceQueue with which instance is registered, or ReferenceQueue.NULL if it was not registered with a queue; next = null.

翻译：Active状态的Reference会受到GC的特别关注，当GC察觉到引用的可达性变化为其它的状态之后，它的状态将变化为Pending或Inactive，到底转化为Pending状态还是Inactive状态取决于此Reference对象创建时是否注册了queue.如果注册了queue，则将添加此实例到pending-Reference list中。 新创建的Reference实例的状态是Active。

> Pending: An element of the pending-Reference list, waiting to be enqueued by the Reference-handler thread.  Unregistered instances are never in this state.

> Pending: queue = ReferenceQueue with which instance is registered;next = this

翻译：在pending-Reference list中等待着被Reference-handler 线程入队列queue中的元素就处于这个状态，未注册ReferenceQueue的实例是不可能到达这一状态。

> Enqueued: An element of the queue with which the instance was registered when it was created.  When an instance is removed from its ReferenceQueue, it is made Inactive.  Unregistered instances are never in this state.

> Enqueued: queue = ReferenceQueue.ENQUEUED; next = Following instance in queue, or this if at end of list.

翻译：当实例被移动到ReferenceQueue中时，Reference的状态为Inactive。没有注册ReferenceQueue的实例不可能到达这一状态的

> Inactive: Nothing more to do. Once an instance becomes Inactive its state will never change again.

> Inactive: queue = ReferenceQueue.NULL; next = this.

翻译：一旦一个实例变为Inactive，则这个状态永远都不会再被改变。

## 参考

- http://blog.csdn.net/u010412719/article/details/52035792
- http://www.cnblogs.com/jabnih/p/6580665.html
- http://blog.csdn.net/ai_xiangjuan/article/details/75042166
- https://leokongwq.github.io/2016/11/15/java-Reference.html