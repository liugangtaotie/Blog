RxJava

## 前言
前些日子，一个百度的同学告诉我RxJava挺好用，然后我就试着看了一下，开始真的很难理解，RxJava学习了好久，根据自己的理解，完成了这篇笔记。不得不说，Rx真的是**碉堡了**的存在！

**那么RxJava为什么难理解?**

我在学习完RxJava以后，觉得最难理解的地方有以下几点

- 定义不清楚：调度器、过滤器、序列、转换、观察者...
- 让人容易混淆的方法名：flatMap、flater、map、subscriber、subscribe、subscribeOn、Observer、observeOn、from、just、create... 说实话，看着就头大，记不住。

## 响应式编程

在学习RxJava和RxAndroid开始之前，我们先需要知道什么是响应式编程，因为RxJava就是响应式编程。

### 定义
- 响应式编程是一种面向数据流和变化传播的编程范式。
- 响应式编程最初是为了简化交互式用户界面的创建和实时系统动画的绘制而提出来的一种方法，但它本质上是一种通用的编程范式。
- 我自己的理解：对事件作出响应。（比如Android上的点击事件，输入监听之类）

### 举例
1. Web前端可能用的比较多（输入、验证码）
2. 用比较通用的举例说明

```
a = b + c;
```

- 在命令式编程中，改变b与c的值，不会影响a。
- 在响应式编程中，改变b与c的值，会影响a。

## RxJava&RxAndroid

### RxJava是什么

```
a library for composing asynchronous and event-based programs using observable sequences for the Java VM
```

简单来说：它就是一个「异步」库

### 特点
- 异步
- 简洁
- 清晰
- 观察者模式

### 基本概念
**注意：不懂观察者模式的请先去学习观察者模式**

#### Observer/Subscriber
**观察者对象：决定了触发事件时，将发生怎样的行为。**

##### 接口实现

```
    Observer<Student> observer = new Observer<Student>() {
        @Override
        public void onCompleted() {
        }

        @Override
        public void onError(Throwable e) {
        }

        @Override
        public void onNext(Student student) {
        }
    };
```

- onNext:执行的方法
- onCompleted:完成 与 onError 方法互斥
- onError:异常 与 onCompleted 方法互斥

##### Subscriber
**实现了 Observer 接口的抽象类。使用Observer到最后也会转换为Subscriber**

```
    Subscriber<Student> subscriber = new Subscriber<Student>() {
        @Override
        public void onCompleted() {
        }

        @Override
        public void onError(Throwable e) {
        }

        @Override
        public void onNext(Student student) {
        }

        @Override
        public void onStart() {
            super.onStart();
        }
    };
```

- 增加了 onStart 方法
- 增加了 Subscriber 所实现的另一个接口 Subscription 的 unsubscribe 方法

#### Observable
**可观察者（被观察者）对象：它决定什么时候触发事件以及触发怎样的事件**

```
    Observable<Student> observable = Observable.create(new Observable.OnSubscribe<Student>() {
        @Override
        public void call(Subscriber<? super Student> subscriber) {
            subscriber.onNext(new Student(1, "Name1"));
            subscriber.onNext(new Student(2, "Name2"));
            subscriber.onNext(new Student(3, "Name3"));
            subscriber.onCompleted();
        }
    });
```

**上述代码等同于**

```
        Observable.just(
                new Student(1, "Name1"),
                new Student(2, "Name2"),
                new Student(3, "Name3"));
```

**也等同于**

```
        Student[] students = {
                new Student(1, "Name1"),
                new Student(2, "Name2"),
                new Student(3, "Name3")};
        Observable.from(students);
```

#### subscribe 订阅
**使用下面这行代码，就能启动了**

```
        observable.subscribe(observer);
        //或者
        observable.subscribe(subscriber);
```

**还可以这么使用**

```
// 自动创建 Subscriber ，并使用 onNextAction 来定义 onNext()
observable.subscribe(onNextAction);
// 自动创建 Subscriber ，并使用 onNextAction 和 onErrorAction 来定义 onNext() 和 onError()
observable.subscribe(onNextAction, onErrorAction);
// 自动创建 Subscriber ，并使用 onNextAction、 onErrorAction 和 onCompletedAction 来定义 onNext()、 onError() 和 onCompleted()
observable.subscribe(onNextAction, onErrorAction, onCompletedAction);
```

**我们以最后一个方法为例**

```
        observable.subscribe(
                new Action1<Student>() {
                    @Override
                    public void call(Student student) {
                        // 等同于onNext方法
                    }
                }, new Action1<Throwable>() {
                    @Override
                    public void call(Throwable throwable) {
                        // 等同于onError方法
                    }
                }, new Action0() {
                    @Override
                    public void call() {
                        // 等同于onCompleted方法
                    }
                });
```

### Scheduler
**线程调度器：这便是RxJava比较NB的地方了**

#### 线程种类

- Schedulers.immediate();//当前线程
- Schedulers.newThread();//新线程
- Schedulers.io();//读写文件、读写数据库、网络信息交互
- Schedulers.computation();//CPU 密集型计算 比如说计算某一个特殊波形的坐标点需要进行大量计算
- AndroidSchedulers.mainThread();// Android中常用的主线程

#### 线程切换方式

- subscribeOn();// 可观察者 Observable 的回调方法发生的线程
- observeOn();// 观察者 subscriber 的回调方法发生的线程

```
        // 被观察者要注册到观察者1，2，3中
        Observable
                // 创建 Observable 可观察者
                .create(new Observable.OnSubscribe<Student>() {
                    @Override
                    public void call(Subscriber<? super Student> subscriber) {
                        // 运行在 Schedulers.io()
                    }
                })
                // 订阅在 IO 线程上
                .subscribeOn(Schedulers.io())
                // 观察在 主 线程上
                .observeOn(AndroidSchedulers.mainThread())
                // 订阅观察者
                .subscribe(new Subscriber<Student>() {
                    @Override
                    public void onCompleted() {
                    }

                    @Override
                    public void onError(Throwable e) {
                    }

                    @Override
                    public void onNext(Student student) {
                        // 运行在 AndroidSchedulers.mainThread()
                    }
                });
```

### Map操作
**变换：就是把事件对象转换成自己目的事件对象**

#### 一般Map

- 没有使用Map的写法

```
        Student students[] = new Student[5];
        Observable
                .from(students)
                .subscribe(new Action1<Student>() {
                    @Override
                    public void call(Student student) {
                    }
                });
```

- 使用Map的写法

```
        Student students[] = new Student[N];
        Observable
                .from(students)
                .map(new Func1<Student, Course>() {
                    @Override
                    public Course call(Student student) {
                        return student.getMajorCourse();//专业课
                    }
                })
                .subscribe(new Action1<Course>() {
                    @Override
                    public void call(Course course) {
                    }
                });
```

上述代码 `// todo function student -> course` 表示我们省略了`Student` 和`Course` 转换的步骤。经过对比，我们会发现Map使逻辑变得更清晰**（注意：Map可以调用多次）**。当然，对于简单需求，你可能反而会觉得有点臃肿和繁琐。

#### flatMap

**同级转换：意思是把多级的转换为一级的转换。**

举例：我们需要对每个学生的每门课程进行一些操作

- 常规写法

```
        Student students[] = new Student[N];
        Observable
                .from(students)
                .subscribe(new Action1<Student>() {
                    @Override
                    public void call(Student student) {
                        Observable
                                .from(student.getAllCourses())
                                .subscribe(new Action1<Course>() {
                                    @Override
                                    public void call(Course course) {
                                    }
                                });
                    }
                });
```

- flapMap写法

```
        Observable
                .from(students)
                .flatMap(new Func1<Student, Observable<Course>>() {
                    @Override
                    public Observable<Course> call(Student student) {
                        return Observable.from(student.getAllCourses());
                    }
                })
                .subscribe(new Action1<Course>() {
                    @Override
                    public void call(Course course) {
                    }
                });
```

发现什么了没有？没错，`flatMap` 使用Observable为每个Course对象创建一个事件，放入到同一个Observable对象中，然后统一在subscribe中处理。相当于把两级的数据，抹平为一级。再说通俗点的话，`Observable.from(students)` 和 `flatMap` 都是类似 `for` 的作用。**（注意：flatMap也可以调用多次，这样就可以把多级数据抹平为一级）** 

#### 源码 lift

原理：map,flatMap变换最后都会使用lift。在 Observable 执行了 lift(Operator) 方法之后，会返回一个新的 Observable，这个新的 Observable 会像一个代理一样，负责接收原始的 Observable 发出的事件，并在处理后发送给 Subscriber。

RxJava 都**不建议**开发者自定义 Operator 来直接使用 lift()，而是建议尽量使用已有的 lift() 包装方法（如 map() flatMap() 等）进行组合来实现需求，因为直接使用 lift() 非常容易发生一些难以发现的错误。

### 操作符 Operator 详解

你以为讲到这里就算完了？No，这不是结束，这只是开始！只能说，你刚看到了RxJava的冰山一角。不得不说，Rx是一个碉堡了的存在，因为这真的是相当强大的一个框架！我们还需要继续挖掘他的其他吊炸天的功能！当然，有兴趣的大牛可以看看RxJava的源码实现！

之前我们的讲解已经涉及到了操作符：map,from,flatMap等等。RxJava中有很多的操作符可用。**重点参考这个：[ReactiveX文档中文翻译](https://mcxiaoke.gitbooks.io/rxdocs/content/)** 

## Follow Me
- [王玉超](https://github.com/yuchao-wang)

## 许可协议
- [署名-非商业性使用-相同方式共享 4.0 国际](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode)

## 参考

- [ReactiveX文档中文翻译](https://mcxiaoke.gitbooks.io/rxdocs/content/)
- [cn-ljb](https://github.com/cn-ljb/rxjava_for_android)
- [大头鬼Bruce](http://blog.csdn.net/lzyzsd/article/category/2767743)
- [扔物线](http://gank.io/post/560e15be2dca930e00da1083)
- [扔物线](https://github.com/rengwuxian/RxJavaSamples)