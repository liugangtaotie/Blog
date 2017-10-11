## Array

Array类提供静态方法来动态创建和访问Java数组，常用方法如下

```
public static Object newInstance(Class<?> componentType, int length)
public static Object newInstance(Class<?> componentType, int... dimensions)
public static native Object get(Object array, int index)
public static native void set(Object array, int index, Object value)
public static native int getLength(Object array)
```

## Arrays

操作数组（比如排序和搜索）的各种方法，常用方法如下

```
public static <T> List<T> asList(T... a) // 返回一个受指定数组支持的固定大小的列表 
public static <T> int binarySearch(T[] a, T key, Comparator<? super T> c)       // 使用二分搜索法来搜索指定数组
public static <T> int binarySearch(T[] a, int fromIndex, int toIndex, long key) // 使用二分搜索法来搜索指定数组
public static <T> T[] copyOf(T[] original, int newLength) // 复制指定长度的数组 
public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) // 复制指定长度的数组
public static <T> T[] copyOfRange(T[] original, int from, int to) // 复制指定范围的数组
public static <T,U> T[] copyOfRange(U[] original, int from, int to, Class<? extends T[]> newType) // 复制指定范围的数组
public static boolean deepEquals(Object[] a1, Object[] a2) // 如果两个指定数组彼此是深层相等 的，则返回 true。
public static String deepToString(Object[] a) // 返回指定数组“深层内容”的字符串表示形式
public static int deepHashCode(Object a[])    // 基于指定数组的“深层内容”返回哈希码
public static boolean equals(Object[] a, Object[] a2) // 如果两个指定的 Objects 数组彼此相等，则返回 true
public static void fill(Object[] a, Object val)       // 将指定的 Object 引用分配给指定 Object 数组的每个元素
public static void fill(Object[] a, int fromIndex, int toIndex, Object val) // 将指定的 Object 引用分配给指定 Object 数组的每个元素
public static int hashCode(Object a[]) // 基于指定数组的内容返回哈希码
public static void sort(Object[] a, int fromIndex, int toIndex) // 根据元素的自然顺序对指定对象数组的指定范围按升序进行排序(根据Comparable接口的compareTo方法)
public static <T> void sort(T[] a, Comparator<? super T> c)
public static <T> void sort(T[] a, int fromIndex, int toIndex, Comparator<? super T> c)
public static <T extends Comparable<? super T>> void parallelSort(T[] a) // 排序算法是一个并行排序合并，将数组分解为子数组，这些子数组本身被排序然后合并 
public static <T> void parallelSort(T[] a, Comparator<? super T> cmp)
public static <T> void parallelSort(T[] a, int fromIndex, int toIndex, Comparator<? super T> cmp)
public static <T> void parallelPrefix(T[] array, BinaryOperator<T> op)
public static <T> void parallelPrefix(T[] array, int fromIndex,int toIndex, BinaryOperator<T> op) 
public static <T> void setAll(T[] array, IntFunction<? extends T> generator)
public static <T> void parallelSetAll(T[] array, IntFunction<? extends T> generator)
public static <T> Spliterator<T> spliterator(T[] array)
public static <T> Spliterator<T> spliterator(T[] array, int startInclusive, int endExclusive)
public static <T> Stream<T> stream(T[] array)
public static <T> Stream<T> stream(T[] array, int startInclusive, int endExclusive)
```

## 参考

- http://tool.oschina.net/apidocs/apidoc?api=jdk-zh
- https://blog.fondme.cn/apidoc/jdk-1.8-google/