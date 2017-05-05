## Java反射-基础篇

自己一直以来其实对Java反射都懵懵懂懂的，一知半解，只知道因为反射，所以Java是个动态语言，而且可以在运行状态中，知道这个类的所有属性和调用所有的方法，也就是所谓的自审。这个其实就是反射机制。

### 定义

简单来说：JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；

### 介绍
- Field类：提供有关类或接口的属性的信息，以及对它的动态访问权限。反射的字段可能是一个类（静态）属性或实例属性，简单的理解可以把它看成一个封装反射类的属性的类。
- Constructor类：提供关于类的单个构造方法的信息以及对它的访问权限。这个类和Field类不同，Field类封装了反射类的属性，而Constructor类则封装了反射类的构造方法。
- Method类：提供关于类或接口上单独某个方法的信息。所反映的方法可能是类方法或实例方法（包括抽象方法）。 这个类不难理解，它是用来封装反射类方法的一个类。
- Class类：类的实例表示正在运行的 Java 应用程序中的类和接口。枚举是一种类，注释是一种接口。每个数组属于被映射为 Class 对象的一个类，所有具有相同元素类型和维数的数组都共享该 Class 对象。
- Object类：每个类都使用 Object 作为超类。所有对象（包括数组）都实现这个类的方法。

### 简单使用

```
public class ReflectModelSuper {
    boolean xx;
    public int aa;
    protected float bb = 3;
    private String cc = "ccccc";
}
```

```
public class ReflectModel extends ReflectModelSuper implements ReflectModelInterface, ReflectModelInterface2 {

    public int a;
    protected float b;
    private String c = "ccc";

    public ReflectModel() {
    }

    public ReflectModel(int a) {
        this.a = a;
    }

    public ReflectModel(int a, float b) {
        this.a = a;
        this.b = b;
    }

    public ReflectModel(int a, float b, String c) {
        this.a = a;
        this.b = b;
        this.c = c;
    }

    @Override
    public String toString() {
        return "ReflectModel{" +
                "a=" + a +
                ", b=" + b +
                ", c='" + c + '\'' +
                '}';
    }

    public int getA() {
        return a;
    }

    public void setA(int a) {
        this.a = a;
    }

    public float getB() {
        return b;
    }

    public void setB(float b) {
        this.b = b;
    }

    public String getC() {
        return c;
    }

    public void setC(String c) {
        this.c = c;
    }
}
```

**实现**

```
package wang.yuchao.java.study.reflect;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class Demo {
    public static void main(String[] args) {
        ReflectModel reflectModel = new ReflectModel();
        Class reflectModelClass = reflectModel.getClass();

        System.out.println("----------------------------");
        System.out.println(reflectModelClass.getName());
        System.out.println(reflectModelClass.getSimpleName());

        try {
            System.out.println("本类的所有属性：（不包括父类）");
            Field[] fa = reflectModelClass.getDeclaredFields();
            for (Field field : fa) {
                System.out.println(field.getName() + " " + field.getType());
            }

            System.out.println("本类的公有属性：（包括父类）");
            Field[] fb = reflectModelClass.getFields();
            for (Field field : fb) {
                System.out.println(field.getName() + " " + field.getType());
            }

            System.out.println("单独属性操作：");
            Field f = reflectModelClass.getDeclaredField("c");
            f.setAccessible(true);//确保私有属性可以访问

            String string = (String) f.get(reflectModel);
            System.out.println("查看某个属性：" + string);

            f.set(reflectModel, "ccc modify");
            System.out.println("修改某个属性：" + reflectModel.toString());

        } catch (Exception e) {
            e.printStackTrace();
        }

        System.out.println("----------------------------");

        try {
            Method[] methods = reflectModelClass.getMethods();
            for (Method method : methods) {
                System.out.println("--> 方法：" + method.getName() + "返回：" + method.getReturnType().getName());
                Class[] parameterTypes = method.getParameterTypes();
                for (Class temp : parameterTypes) {
                    System.out.println("参数：" + temp.getName());
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }


        System.out.println("----------------------------");

        try {
            Constructor[] constructors = reflectModelClass.getConstructors();
            for (Constructor constructor : constructors) {
                System.out.println("构造方法" + constructor.toGenericString());
                Class[] parameterTypes = constructor.getParameterTypes();
                for (Class temp : parameterTypes) {
                    System.out.println("参数：" + temp.getName());
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

        System.out.println("----------------------------");

        try {
            Class[] interfaces = reflectModelClass.getInterfaces();
            for (Class anInterface : interfaces) {
                System.out.println("接口：" + anInterface.getName());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

        System.out.println("----------------------------");

        try {
            Class superclass = reflectModelClass.getSuperclass();
            System.out.println("父类：" + superclass.getName());
        } catch (Exception e) {
            e.printStackTrace();
        }

        System.out.println("----------------------------");

        try {
            Class<?> clz = Class.forName("wang.yuchao.java.study.reflect.ReflectModel");
            Object object = clz.newInstance();
            Method method1 = clz.getMethod("getA");
            System.out.println("getA:" + method1.invoke(object));

            Method method2 = clz.getMethod("setA", int.class);
            method2.invoke(object, 5);
            System.out.println("getA:" + method1.invoke(object));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### PS(2016/05/17)
- 还可以通过反射获取注解信息

### 参考
- [博客园](http://www.cnblogs.com/forlina/archive/2011/06/21/2085849.html) 
- [CODEKK](http://a.codekk.com/detail/Android/Mr.Simple/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20Java%20%E5%8F%8D%E5%B0%84%20Reflection)