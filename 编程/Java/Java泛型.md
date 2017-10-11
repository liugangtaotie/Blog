
## 前言

Java是一种强类型的语言，定义一个变量时需要指明其类型。Object是所有类的基类，在Java 1.5之前，为了让类具有通用性，通常使用Object来实现参数的任意化。存数据时，将类XClass向上转换为Object基类；取数据时，将Object向下转型为XClass；但是向下强制转换时，需要知道类型XClass，存在着极大的安全隐患。

**泛型**（Generic type 或 generics）是Java 1.5之后的扩展，以支持类的参数化，即类本身也可以作为一种参数。也可以把泛型看作是使用参数化类型时指定的类型的一个占位符，就像方法的形式参数是运行时传递的值的占位符一样。

## 用法

### 常见用法

语法格式是：`<T1,T2,T3,...,Tn>`，示例如下

```
public class NormalFoo<T> {

    private T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }

    public static void main(String[] args) {
        NormalFoo<String> normalFoo0 = new NormalFoo<>();
        normalFoo0.setT("wangyuchao");
        System.out.println(normalFoo0.getT());

        NormalFoo<Double> normalFoo1 = new NormalFoo<>();
        normalFoo1.setT(new Double(1000));
        System.out.println(normalFoo1.getT());
    }
}
```

### 接口实现

```
public interface FooInterface<T> {
    void setT(T t);

    T getT();
}
public class FooImplements<T> implements FooInterface<T> {
    @Override
    public void setT(T t) {
    }

    @Override
    public T getT() {
        return null;
    }
}
```

### 静态方法

注意：同一个泛型类不可能同时存在于静态方法和非静态方法。

```
    public static <T> T getT(T t) {
        return t;
    }
```

### 限制泛型（继承）

```
public class FooSub<T extends FooFather> {
}
```

### 通配符类型用法

**<?>**: 基本等价于<Object>，可省略

```
    public ArrayList<?> getArrayList() {
        ArrayList<?> collection = null;
        collection = new ArrayList<Number>();
        collection = new ArrayList<Integer>();
        collection = new ArrayList<String>();
        return collection;
    }
```

**List<?>与List<Object>区别**

```
List<?> list = new ArrayList<String>();       // 正确，可以赋值
List<Object> list = new ArrayList<String>();  // 错误，不可赋值
```

**extends**: `<? extends XClass>` 只能是XClass类以及XClass类的子类。

```
    public ArrayList<? extends Number> getArrayList() {
        ArrayList<? extends Number> collection = null;
        collection = new ArrayList<Number>();
        collection = new ArrayList<Short>();
        collection = new ArrayList<Integer>();
        return collection;
    }
```

**<T extends XClass>与<? extends XClass>区别**

1. `T` 是泛型 `?` 是通配符
2. `T` 可以进行某些判断以及操作 `?`后续使用时可以是任意的
3. `T` 用于筛选符合要求的某种类型 `?` 用于筛选未知的某种类型

```
<T extends XClass>  // XClass以及自身的任一子类（ 注意:没有<T super XClass>）
<? extends XClass>  // 解决类型边界问题，上边界通配符，XClass 可以是泛型 T
<? super XClass>    // 解决类型边界问题，下边界通配符，XClass 可以是泛型 T 
```

**super**: `<? super XClass>` 只能是XClass类以及XClass的父类。

```
    public ArrayList<? super Integer> getArrayList() {
        ArrayList<? super Integer> collection = null;
        collection = new ArrayList<Object>();
        collection = new ArrayList<Number>();
        collection = new ArrayList<Integer>();
        return collection;
    }
```

**范例如下**

```
public class DemoForT {

    // 食物
    public static class Food {
    }

    // 水果
    public static class Fruit extends Food {
    }

    // 苹果
    public static class Apple extends Fruit {
    }

    // 盘子
    public static class Plate<T> {
        private T t;

        public Plate(T t) {
            this.t = t;
        }

        public T getT() {
            return t;
        }

        public void setT(T t) {
            this.t = t;
        }

        // 参数传递：打印新盘子的东西 T 或 T 的子类
        public void print1(Plate<? extends T> newPlate) {
            System.out.println(newPlate.t);
        }

        // 参数传递：打印新盘子的东西 T 或 T 的父类
        public void print2(Plate<? super T> newPlate) {
            System.out.println(newPlate.t);
        }

    }

    public static void main(String[] args) {
        Food food = new Food();

        Fruit fruit = new Fruit();

        Apple apple = new Apple();

        // 通配符
        Plate<?> a = new Plate<>(food);
        Plate<?> b = new Plate<>(fruit);
        Plate<?> c = new Plate<>(apple);

        // Fruit 的任意子类
        Plate<? extends Fruit> plate1 = new Plate<>(apple);

        // Fruit 的任意父类
        Plate<? super Fruit> plate2 = new Plate<>(food);

        // 参数传递
        Plate<Fruit> plate3 = new Plate<>(fruit);

        // 传递 Fruit 的任一子类
        plate3.print1(plate1);       // success
        // plate3.print1(plate2);    // error

        // 传递 Fruit 的任一父类
        // plate3.print2(plate1);    // error
        plate3.print2(plate2);       // success
    }
}
```

### 数组

```
HashMap<String,String> [] maps = (HashMap<String, String>[]) new HashMap[10]; // 正确用法
HashMap<String,String>[] maps = new HashMap<String, String>[10];              // 错误用法
```

## 类型擦除

> 注意：在泛型代码内部，无法获得任何有关泛型参数类型的信息

Java 中的泛型只存在于编译期，在将 Java 源文件编译完成 Java 字节代码中是不包含泛型中的类型信息的。使用泛型的时候加上的参数类型XClass，会被编译器在编译的时候去掉，全部转化为Object类型，然后再由编译器自动强制转换为传入的参数类型XClass。

**下面我们分析一下字节码来验证一下这个结论**

要分析的java文件如下

```
public class WipeDemo<T> {
    private T t;

    public WipeDemo(T t) {
        this.t = t;
    }

    public T getT() {
        return t;
    }

    public static void main(String[] args) {
        WipeDemo<String> demo = new WipeDemo<>("Hello");
        String string = demo.getT();
        System.out.println(string);
    }
}
```

编译`javac WipeDemo.java`完成以后，使用`javap -c -s WipeDemo`查看一下字节码

```
警告: 二进制文件WipeDemo包含wang.yuchao.java.study.generic.WipeDemo
Compiled from "WipeDemo.java"
public class wang.yuchao.java.study.generic.WipeDemo<T> {
  public wang.yuchao.java.study.generic.WipeDemo(T);
    descriptor: (Ljava/lang/Object;)V
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: aload_0
       5: aload_1
       6: putfield      #2                  // Field t:Ljava/lang/Object;
       9: return

  public T getT();
    descriptor: ()Ljava/lang/Object;
    Code:
       0: aload_0
       1: getfield      #2                  // Field t:Ljava/lang/Object;
       4: areturn

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    Code:
       0: new           #3                  // class wang/yuchao/java/study/generic/WipeDemo
       3: dup
       4: ldc           #4                  // String Hello
       6: invokespecial #5                  // Method "<init>":(Ljava/lang/Object;)V
       9: astore_1
      10: aload_1
      11: invokevirtual #6                  // Method getT:()Ljava/lang/Object;
      14: checkcast     #7                  // class java/lang/String
      17: astore_2
      18: getstatic     #8                  // Field java/lang/System.out:Ljava/io/PrintStream;
      21: aload_2
      22: invokevirtual #9                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      25: return
}
```

通过分析上述字节码，才得到开头类型擦除的结论。

由于存在类型擦除，因此不能实例化类型变量:`new T();`、`new T[n]`、`T.class`。否则泛型T编译完成以后会直接变成Object类型。不过可以使用如下方法代替。

```
T [] arrays = (T[]) new Object[N];
```

## 参考

- [博客园](http://www.cnblogs.com/absfree/p/5270883.html)
- [Java核心技术](https://book.douban.com/subject/3146174/)
- [知乎](https://www.zhihu.com/question/20400700/answer/117464182?utm_source=qq&utm_medium=social)