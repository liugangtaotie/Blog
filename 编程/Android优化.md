
## 应用瘦身

- 首先打包 `xxx.apk`
- `Build -> Analyze APK` 导入 `xxx.apk` 后会发现如下图
- 然后我们会大致分析一下应用的文件大小是否正常，将未用到的大文件清理
- 点击DEX文件，还可以查看类个数和方法个数，这里如果应用达到了64k的限制，那么我们则需要将应用拆分，插件化处理。
- 点击DEX文件，我们会看到包名和类名，分析靠前的包是否需要。（有时候我们会引用到相似功能的第三方库，但是我们只用了一个，删除不必要的类库，当然也可以直接在gradle里面进行分析）
- 想知道跟老版本的区别，可以点击右上角的 `Compare with`
- 去掉无用资源以及类库后，最后重新打包即可 

![应用瘦身](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-analyze.png)

## 代码分析
- `Analyze -> Inspect Code` 我们会发现下图
- 不要被问题数吓到，可能有很多是类库自己的问题，我们只需要分析自己应用的问题即可。
- 大部分都是比较简单的容易处理的小问题，然后有些提示其实也没必要改动，一般就是用来优化代码执行速率以及减少内存开销的一些问题。
- 最方便的一点就是，点开每一个问题,右侧都给出了问题的定位、描述、解决办法。我们能从这些提示中学到不少关于代码优化的小技巧呢。

![代码分析](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-inspect-code.png)

## 内存优化

**内存优化是Android应用优化最重要的一部分**

### 内存分配
- **静态区**：主要存放静态数据、全局 static 数据和常量。这块内存在程序编译时就已经分配好，并且在程序整个运行期间都存在。
- **栈区** ：当方法被执行时，方法体内的局部变量都在栈上创建，并在方法执行结束时这些局部变量所持有的内存将会自动被释放。因为栈内存分配运算内置于处理器的指令集中，效率很高，但是分配的内存容量有限。
- **堆区** ：又称动态内存分配，通常就是指在程序运行时直接 new 出来的内存。这部分内存在不使用时将会由 Java 垃圾回收器来负责回收。

### 内存泄露
- **直观表述：**只要长的生命周期引用一个比他生命同期短的对象就可能内存泄漏
- **根本原因：**内存中有些对象的引用还存在，但是却没有什么用处，GC没有办法释放，因此还占用内存。
- **表现形式：**A对象引用B对象，A对象的生命周期（t1-t4）比B对象的生命周期（t2-t3）长的多。当B对象没有被应用程序使用之后，A对象仍然在引用着B对象。这样，垃圾回收器就没办法将B对象从内存中移除，从而导致内存问题。

### 常见情景
- **集合类**:Map or List
```
    public static void main(String[] args) {
        ArrayList<Student> arrayList = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            Student student = new Student();
            student.name = i + "";
            arrayList.add(student);
            if (i == 2) {
                student = null;//没有释放内存，造成内存泄露
            }
        }
        System.out.println(arrayList.toString());
        arrayList.set(3, null);// 正确释放内存
        System.out.println(arrayList.toString());
    }
    
    // 结果如下
    [Student{name='0'}, Student{name='1'}, Student{name='2'}, Student{name='3'}, Student{name='4'}]
    [Student{name='0'}, Student{name='1'}, Student{name='2'}, null, Student{name='4'}]
```

- **非静态内部类创建静态实例**:因为非静态内部类默认会持有外部类的引用，而该非静态内部类又创建了一个静态的实例，该实例的生命周期和应用的一样长，这就导致了该静态实例一直会持有该Activity的引用，导致Activity的内存资源不能正常回收。正确做法是将`Test`内部类抽取出来或者设置为静态内部类。
```
public class Activity {
    private static Test test = null;

    class Test {
    }

    public void onCreate() {
        if (test == null) {
            test = new Test();
        }
    }
}
```

- **匿名内部类**:匿名内部类会持有外部类的一个引用，因此不要再将这个引用，传入异步线程。否则一定会造成内存泄露。

- **static**:跟应用的生命周期是一样长。永远不要创建静态的 Context 或 View 对象，或者将二者存储于静态变量中。

- **Context**:能传Context尽量不要传 Activity 。避免在 Activity 或 Fragment 之外传递 Context 对象。尽量使用 ApplicationContext 而不是Activity;

- **异步任务**:Handler+Thread 或者 AsyncTask 。
 + 不要在 AsyncTasks(异步任务)或后台线程中存储指向 activities 的强引用。Activity 可能会关闭，但是 AsyncTask 会继续执行，一直保存着对该 activity 的引用。
 + 如果我们一定需要传入 Activity （Dialog，Progress 之类）来进更改UI，可以使用 WeakReference 持有 Activity 的引用，每次使用前注意判空即可。 
 + Handler 造成内存泄露很常见，推荐使用静态内部类 + WeakReference 这种方式。具体请看[WeakHandler](https://github.com/badoo/android-weak-handler)
 + Handler 还可以使用 removeCallbacks 和 removeMessages 移除

- **资源未关闭**:对于使用了 Receiver Stream Observer File Cursor 等资源，应该在Activity销毁时及时关闭或者注销，回收内存。

## Android Monitors

![Monitors](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-monitor.png)

点击`Dump Java Heap` 会自动打开 .hprof文件,可以查看如下图所示

![hprof文件分析](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-hprof.png)

![hprof文件分析](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-hprof2.png)

![hprof文件分析](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-hprof3.png)

那么我们怎么才能发现内存泄露呢？这只是显示了一些基础的保留堆栈、总栈、层次、深度之类的参数，我们只能分析比较浅显容易看出来的内存泄露，比如某一个Bitmap占用了大量的内存，然后就可以 `jump to source` 。如果想要详细分析应用内存，这时就需要用到MAT了。

## MAT 分析跟踪
- [下载地址](http://www.eclipse.org/mat/downloads.php)
- 首先将 .hprof 文件导出为标准文件 ,然后使用 MAT 打开,选择 `leak suspect report` ,

![hprof文件分析](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-hprof4.png)

![MAT](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-mat.png)

![MAT](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-mat2.png)

![MAT](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-mat3.png)

可以根据上图中引用个数判断是否内存泄露，如果引用个数过多，可能就会造成内存泄露。我们点击`Histogram` 查看一下 `ArrayList`

![MAT](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-mat4.png)

![MAT](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-mat5.png)

通过对包名`weiboyi`的搜索发现，`util` 包中 GsonUtil 可能会有内存泄露，因为引用数量太多了。

下面我们再搜索一下`activity`,看是否存在`Activity`的泄露。可能我这运行次数比较少，目前还没有发现。

![MAT](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-mat6.png)

**以上都仅仅是比较简单的用法，MAT的更多功能只能慢慢探索。**

## LeakCanary

如果你厌倦了上述的检测内存泄露的繁琐方式，那么现在有个开源类库[LeakCanary](https://github.com/square/leakcanary)，可以直接让内存泄露无所遁形。

具体的说明请看官网吧，[LeakCanary中文说明在这](https://www.liaohuqiu.net/cn/posts/leak-canary-read-me/)

## 参考
- [阿里云](https://yq.aliyun.com/articles/3009)
- [知乎](https://zhuanlan.zhihu.com/p/24262346)