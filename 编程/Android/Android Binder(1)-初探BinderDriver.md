## 背景知识

**进程隔离**

> 进程隔离是为保护操作系统中进程互不干扰而设计的一组不同硬件和软件的技术。这个技术是为了避免进程A写入进程B的情况发生。 进程的隔离实现，使用了虚拟地址空间。进程A的虚拟地址和进程B的虚拟地址不同，这样就防止进程A将数据信息写入进程B。

操作系统的不同进程之间，数据不共享；对于每个进程来说，它都天真地以为自己独享了整个系统，完全不知道其他进程的存在；(有关虚拟地址，请自行查阅）因此一个进程需要与另外一个进程通信，需要某种系统机制才能完成。

**用户空间/内核空间**

> Linux虚拟内存的大小为2^32（在32位的x86机器上），内核将这4G字节的空间分为两部分。最高的1G字节（从虚地址0xC0000000到0xFFFFFFFF）供内核使用，称为“内核空间”。而较低的3G字节（从虚地址0x00000000到0xBFFFFFFF），供各个进程使用，称为“用户空间”。因为每个进程可以通过系统调用进入内核，因此，Linux内核空间由系统内的所有进程共享。于是，从具体进程的角度来看，每个进程可以拥有4G字节的虚拟地址空间(也叫虚拟内存).

用户空间不是进程共享的，而是进程隔离的。每个进程最大都可以有3GB的用户空间。一个进程对其中一个地址的访问，与其它进程对于同一地址的访问绝不冲突。

**系统调用/内核态/用户态**

从逻辑上抽离出用户空间和内核空间；但是不可避免的的是，总有那么一些用户空间需要访问内核的资源；比如应用程序访问文件，网络是很常见的事情，怎么办呢？

> Kernel space can be accessed by user processes only through the use of system calls.

用户空间访问内核空间的唯一方式就是系统调用；通过这个统一入口接口，所有的资源访问都是在内核的控制下执行，以免导致对用户程序对系统资源的越权访问，从而保障了系统的安全和稳定。用户软件良莠不齐，要是它们乱搞把系统玩坏了怎么办？因此对于某些特权操作必须交给安全可靠的内核来执行。

**内核模块/驱动**

通过系统调用，用户空间可以访问内核空间，那么如果一个用户空间想与另外一个用户空间进行通信怎么办呢？很自然想到的是让操作系统内核添加支持，这里就涉及到进程间的通信了。

内核可以访问A和B的所有数据；所以，最简单的方式是通过内核做中转；假设进程A要给进程B发送数据，那么就先把A的数据copy到内核空间，然后把内核空间对应的数据copy到B就完成了；用户空间要操作内核空间，需要通过系统调用；刚好，这里就有两个系统调用：`copy_from_user`, `copy_to_user`。

**进程间通信(IPC)**(RPC：远程过程调用)

Linux下的通信手段基本上是从Unix平台上的进程通信手段继承而来的。而对Unix发展做出重大贡献的两大主力AT&T的贝尔实验室及BSD（加州大学伯克利分校的伯克利软件发布中心）在进程间通信方面的侧重点有所不同。前者对Unix早期的进程间通信手段进行了系统的改进和扩充，形成了“system V IPC”，通信进程局限在单个计算机内；后者则跳过了该限制，形成了基于套接口（socket）的进程间通信机制。由于Unix版本的多样性，电子电气工程协会（IEEE）开发了一个独立的Unix标准，这个新的ANSI Unix标准被称为计算机环境的可移植性操作系统界面（PSOIX）。现有大部分Unix和流行版本都是遵循POSIX标准的，而Linux从一开始就遵循POSIX标准；

Linux则把两者继承了下来，如图示：![IPC历史](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-binder-01.png)

Linux存在着多种IPC方式:管道、共享内存、消息队列、套接字、信号、信号量，这些传统的Linux通信机制都是内核支持的；（Linux下IPC机制详见[参考1](https://www.ibm.com/developerworks/cn/linux/l-ipc/)、[参考2](http://blog.csdn.net/gatieme/article/details/50908749)）

**但是Binder并不是Linux内核的一部分，它是怎么做到访问内核空间的呢**？

Linux的动态可加载内核模块（Loadable Kernel Module，LKM）机制解决了这个问题；模块是具有独立功能的程序，它可以被单独编译，但不能独立运行，它在运行时被链接到内核作为内核的一部分在内核空间运行，有点类似Android应用的插件化、热更新之类的技术。这样，Android系统可以通过添加一个内核模块运行在内核空间，用户进程之间的通过这个模块作为桥梁，就可以完成通信了。

**Binder驱动**

在Android系统中，这个运行在内核空间的，负责各个用户进程通过Binder通信的内核模块叫做Binder驱动;

> 驱动程序一般指的是设备驱动程序（Device Driver），是一种可以使计算机和设备通信的特殊程序。相当于硬件的接口，操作系统只有通过这个接口，才能控制硬件设备的工作；

驱动就是操作硬件的接口，为了支持Binder通信过程，Binder使用了一种"伪硬件"，因此这个模块被称之为驱动。

**那Android为什么自己创要一个新的IPC机制？它有什么独特的地方呢？**

- 采用C/S通信模式
- 传输性能更高。对比Linux的IPC机制
 + socket：是一个通用接口，导致其传输效率低，开销大；
 + 管道和消息队列：因为采用存储转发方式，所以至少需要拷贝2次数据，Binder只拷贝一次；
 + 共享内存：虽然在传输时没有拷贝数据，但其控制机制复杂（比如跨进程通信时，需获取对方进程的pid，得多种机制协同操作）
- 安全性更高。Linux的IPC机制在本身的实现中，并没有安全措施，得依赖上层协议来进行安全控制。而Binder机制的UID/PID是由Binder机制本身在内核空间添加身份标识，安全性高；并且Binder可以建立私有通道，这是linux的通信机制所无法实现的 （Linux访问的接入点是开放的）

**数据载体 Parcel（包裹）**

`Parcel`是一个负责存放数据的容器，将实现了`Parcelable`接口的对象数据序列化转为字节流，这样就可以在在跨进程通信中，把进程A中占据的内存相关数据打包起来，然后寄送到进程B中，由B在自己的进程空间中复现这个对象。凡是经过Parcel包装后的数据都可以在进程间通信中进行服务端和数据端的数据交互，AIDL中也用到了Parcel进行数据封装。 

**Parcelable 接口是Android下的序列化接口，那么我们为什么不用 Serializable 接口呢？**

1. Serializable 在序列化的时候会产生大量的临时变量，通过IO流的形式在硬盘上读写，性能不如 Parcelable。
2. Parcelable 以 IBinder 为接口，内存使用效率比 Serializable 高，使用C++和JNI在共享内存中实现读写数据流，4字节对齐原则，效率提高10倍以上。

## 通信模型

**Android中Binder机制类比:TCP/IP**

```
Binder Driver  --> 路由器
ServiceManager --> DNS
Binder Client  --> 客户端
Binder Server  --> 服务器
Client向DNS查询结果返回IP给Client，然后Client就可以跟Server通信了。Client 与 Server 通信的基础是路由器
```

**Binder 是怎么实现进程间通信呢？（注意：下面ServiceManager统称为SM）**

简单来说：首先Server向SM注册，然后Client向SM查询，然后SM经过查询把结果返回给Client。Client与SM、Server与SM之间都是通过Binder处理的。

1. Server 进程向SM注册，告诉SM自己叫 wang ，有个 object 对象，有一个 add 方法
2. Client 向SM查询一个名叫 wang 进程里面的 object 对象，得到一个经过 Binder 包装过的代理对象 proxy。
3. proxy 执行 add 方法，此时，Binder 通知 Server 进程执行 object 的 add 方法，并且将结果返回给 Client。

![Binder框架图](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-binder-03.png)

**Binder数据映射及复制过程**

1. Binder 通过`binder_open()`打开binder驱动
2. Binder 调用`copy_from_user()`把进程A中的数据复制到虚拟内存空间中。
3. Binder 通过`binder_proc->buffer`指针，经过虚拟内存转换后，指向一块物理内存。
4. Binder 通过`binder_mmap()`把设备指定的物理内存块直接映射到进程B应用内存的内存空间中。

**注意：**

第一：上述 Binder 的数据读写由 `binder_ioctl` 完成，它承担了 Binder 驱动的大部分业务。
第二：我们会发现少了一次拷贝过程 `copy_to_user()`。

**Binder 有哪几种**
- Binder 实体对象： 就是binder服务的提供者，一个提供binder服务的类必须继承BBinder类（native层），因此binder实体对象又叫做BBinder对象。
- Binder 引用对象： 是binder服务提供者在客户端进程的代表，每个引用对象类型必须继承BpBinder类（native层），因此binder引用对象有叫做BpBinder对象。
- Binder 代理对象： 代理对象简单理解为内聚了一个Binder引用对象，因此可以通过这个内聚的引用对象发起RPC调用。（可以有多个不同的代理对象，但却内聚了同一个引用对象）
- IBinder对象： BBinder和BpBinder都是继承自IBinder.因此binder实体对象和binder引用对象都可以称为IBinder对象。可以通过`IBinder.queryLocalInterface()`方法来判断到底是binder实体对象还是binder引用对象。

![Binder框架图](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-binder-02.jpg)

**ServiceManager**

SM 本身也是一个 BinderServer（在Android系统中，Binder Server 的另外一个常见叫法是 `XXX Service`）

SM 是一个事件驱动的程序循环框架（有点类似 Looper），不断循环的从Binder驱动中，读取消息，处理消息。

为了简化流程，隐藏了SM这一部分驱动进行的操作；实际上，由于SM与Server通常不在一个进程，Server进程向SM注册的过程也是跨进程通信，驱动也会对这个过程进行暗箱操作：SM中存在的Server端的对象实际上也是代理对象，后面Client向SM查询的时候，驱动会给Client返回另外一个代理对象。Sever进程的本地对象仅有一个，其他进程所拥有的全部都是它的代理。一句话总结就是：Client进程只不过是持有了Server端的代理；代理对象协助驱动完成了跨进程通信。

## 感谢

- [DemoForAIDL](https://github.com/yuchao-wang/DemoForAIDLServer)（使用Binder调用远程服务）
- http://gityuan.com/2016/09/04/binder-start-service/
- http://wangkuiwu.github.io/2014/09/01/Binder-Introduce/
- http://qiangbo.space/2017-01-15/AndroidAnatomy_Binder_Driver/
- http://www.cloudchou.com/android/post-507.html
- http://www.iloveandroid.net/2016/09/07/android_binder_1/
- http://weishu.me/2016/01/12/binder-index-for-newer/