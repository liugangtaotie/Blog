### 整理

**Integer 缓存**

```
public class Demo {
    public static void main(String[] args) {
        //它缓存了从-128到127
        Integer a = 1000, b = 1000;
        System.out.println(a == b);//false
        Integer c = 100, d = 100;
        System.out.println(c == d);//true
    }
}
```

**static**

static方法加锁锁住的是类，非static方法加锁锁住的是对象

**比较对象相等**

需要重写equals方法，如果用到了集合类，则需要重写hashCode方法，因为很多集合类比较对象时是先比较hashCode是否相等，如果相等再比较效率比较低的equals方法。

**Java 字符串编码**([参考](http://blog.csdn.net/tianjf0514/article/details/7854624))

```
    public static void main(String[] args) throws Exception {
        String str = "王玉超abc";
        System.out.println(str.length());//6
        System.out.println(str.toCharArray().length);//6
        System.out.println(str.getBytes("ISO-8859-1").length);//1*6
        System.out.println(str.getBytes("GBK").length);//2*3+3
        System.out.println(str.getBytes("UTF-8").length);//3*3+3

        // 大端模式：FEFF  小端模式：FFFE
        // 在Java中直接使用Unicode转码时会按照加BOM的UTF-16小端模式BE拆分。
        // 在Java中使用UTF-16转码,会默认采用带有BOM的UTF-16大端模式LE拆分。
        // 下面两个结果是：2+2*6。前面两个字节代表大端模式还是小端模式
        System.out.println(str.getBytes("Unicode").length);
        System.out.println(str.getBytes("UTF-16").length);
    }
```