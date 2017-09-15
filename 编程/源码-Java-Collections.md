## Collections

可以理解为操作集合的帮助类，里面集成了很多集合操作的常用方法。

下面是常用的一些方法

```
public static <T extends Comparable<? super T>> void sort(List<T> list) // 根据元素的自然顺序 对指定列表按升序进行排序
public static <T> void sort(List<T> list, Comparator<? super T> c)      // 根据元素的自然顺序 对指定列表按升序进行排序
public static <T> int binarySearch(List<? extends T> list, T key, Comparator<? super T> c)
public static void reverse(List<?> list) // 反转指定列表中元素的顺序
public static void shuffle(List<?> list) // 使用默认随机源对指定列表进行置换
public static void shuffle(List<?> list, Random rnd) // 使用默认随机源对指定列表进行置换
public static void swap(List<?> list, int i, int j)  // 在指定列表的指定位置处交换元素
public static <T> void fill(List<? super T> list, T obj) // 使用指定元素替换指定列表中的所有元素
public static <T> void copy(List<? super T> dest, List<? extends T> src) // 将所有元素从一个列表复制到另一个列表
public static <T extends Object & Comparable<? super T>> T min(Collection<? extends T> coll) // 根据元素的 自然顺序 返回给定 collection 的最小元素
public static <T> T min(Collection<? extends T> coll, Comparator<? super T> comp)            // 根据元素的 自然顺序 返回给定 collection 的最小元素
public static <T extends Object & Comparable<? super T>> T max(Collection<? extends T> coll) // 根据元素的 自然顺序 返回给定 collection 的最大元素
public static <T> T max(Collection<? extends T> coll, Comparator<? super T> comp)            // 根据元素的 自然顺序 返回给定 collection 的最大元素
public static void rotate(List<?> list, int distance) // 根据指定的距离轮换指定列表中的元素
public static <T> boolean replaceAll(List<T> list, T oldVal, T newVal) // 替换所有元素
public static int indexOfSubList(List<?> source, List<?> target)       // 返回指定源列表中第一次出现指定目标列表的起始位置
public static int lastIndexOfSubList(List<?> source, List<?> target)   // 返回指定源列表中最后一次出现指定目标列表的起始位置
// 不可修改视图
public static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c) // 返回指定 collection 的不可修改视图
public static <T> Set<T> unmodifiableSet(Set<? extends T> s)                      // 返回指定 set 的不可修改视图
public static <T> SortedSet<T> unmodifiableSortedSet(SortedSet<T> s)              // 返回指定有序 set 的不可修改视图
public static <T> NavigableSet<T> unmodifiableNavigableSet(NavigableSet<T> s)     // 返回指定 NavigableSet的不可修改视图
public static <T> List<T> unmodifiableList(List<? extends T> list)                // 返回指定列表的不可修改视图
public static <K,V> Map<K,V> unmodifiableMap(Map<? extends K, ? extends V> m)     // 返回指定映射的不可修改视图
public static <K,V> SortedMap<K,V> unmodifiableSortedMap(SortedMap<K, ? extends V> m)          // 返回指定排序映射的不可修改视图
public static <K,V> NavigableMap<K,V> unmodifiableNavigableMap(NavigableMap<K, ? extends V> m) // 返回指定 NavigableSet的不可修改视图
// 以下都是同步（线程安全）的一些方法
public static <T> Collection<T> synchronizedCollection(Collection<T> c)
public static <T> Set<T> synchronizedSet(Set<T> s)
public static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s)
public static <T> NavigableSet<T> synchronizedNavigableSet(NavigableSet<T> s)
public static <T> List<T> synchronizedList(List<T> list)
public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)
public static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V> m)
public static <K,V> NavigableMap<K,V> synchronizedNavigableMap(NavigableMap<K,V> m)
// 动态类型安全视图
public static <E> Collection<E> checkedCollection(Collection<E> c, Class<E> type) // 返回指定 collection 的一个动态类型安全视图
public static <E> Queue<E> checkedQueue(Queue<E> queue, Class<E> type)
public static <E> Set<E> checkedSet(Set<E> s, Class<E> type)
public static <E> SortedSet<E> checkedSortedSet(SortedSet<E> s,
public static <E> NavigableSet<E> checkedNavigableSet(NavigableSet<E> s,
public static <E> List<E> checkedList(List<E> list, Class<E> type)
public static <K, V> Map<K, V> checkedMap(Map<K, V> m, Class<K> keyType, Class<V> valueType)
public static <K,V> SortedMap<K,V> checkedSortedMap(SortedMap<K, V> m, Class<K> keyType, Class<V> valueType)
public static <K,V> NavigableMap<K,V> checkedNavigableMap(NavigableMap<K, V> m, Class<K> keyType, Class<V> valueType)
// 返回各种空迭代器和集合
public static <T> Iterator<T> emptyIterator()
public static <T> ListIterator<T> emptyListIterator()
public static <T> Enumeration<T> emptyEnumeration()
public static final <T> Set<T> emptySet()
public static <E> SortedSet<E> emptySortedSet()
public static <E> NavigableSet<E> emptyNavigableSet()
public static final <T> List<T> emptyList()
public static final Map EMPTY_MAP = new EmptyMap<>();
public static final <K,V> Map<K,V> emptyMap()
public static final <K,V> SortedMap<K,V> emptySortedMap()
public static final <K,V> NavigableMap<K,V> emptyNavigableMap()
// 返回一个只包含指定对象的不可变集
public static <T> Set<T> singleton(T o)      // 返回一个只包含指定对象的不可变集
public static <T> List<T> singletonList(T o) // 返回一个只包含指定对象的不可变列表
public static <K,V> Map<K,V> singletonMap(K key, V value)
public static <T> List<T> nCopies(int n, T o)
// 返回一个比较器，它对实现Comparable接口的对象集合施加与自然排序相反的比较
public static <T> Comparator<T> reverseOrder()
public static <T> Comparator<T> reverseOrder(Comparator<T> cmp)
public static <T> Enumeration<T> enumeration(final Collection<T> c) // 返回指定集合的枚举
public static <T> ArrayList<T> list(Enumeration<T> e)               // 返回指定列表的枚举
public static int frequency(Collection<?> c, Object o)              // 返回指定 collection 中等于指定对象的元素数
public static boolean disjoint(Collection<?> c1, Collection<?> c2)  // 如果两个指定 collection 中没有相同的元素，则返回 true
public static <T> boolean addAll(Collection<? super T> c, T... elements) // 将所有指定元素添加到指定 collection 中
public static <E> Set<E> newSetFromMap(Map<E, Boolean> map)              // 返回指定映射支持的 set
public static <T> Queue<T> asLifoQueue(Deque<T> deque)                   // 以后进先出 (Lifo) Queue 的形式返回某个 Deque 的视图
```

## 参考

- http://tool.oschina.net/apidocs/apidoc?api=jdk-zh
- https://blog.fondme.cn/apidoc/jdk-1.8-google/