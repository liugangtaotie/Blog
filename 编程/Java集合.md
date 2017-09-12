## 框架

**分类**

主要分为 Map 和 Collection 两大类别。

**简介**

Map：key到value的映射表
Collection：集合根接口。有些有序，有些无序。有些重复，有些重复。Collection没有直接的实现，而只有它的子接口的对应的实现。
Set：不包含重复元素
List：有序集合，可包含重复元素
Queue：队列。
Deque：双向队列。

**根据接口分类**

- Map : EnumMap、IdentityHashMap、HashMap、LinkedHashMap、WeakHashMap、TreeMap
- List : ArrayList、LinkedList
- Set : HashSet、LinkedHashSet、TreeSet
- Queue : PriorityQueue、LinkedList、ArrayQueue

**根据底层数据结构分类**

- 数组：EnumMap、ArrayList、ArrayQueue
- 链表：LinkedHashSet、LinkedList、LinkedHashMap
- HashTable : HashMap、HashSet、LinkedHashMap、LinkedHashSet、WeakHashMap、IdentityHashMap
- 红黑树：TreeMap、TreeSet
- 二叉堆：PriorityQueue

**框架图**

![框架图](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/java-collection.jpg)

![框架图](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/java-collection2.png)

## HashMap

**源码实现**

HashMap 是典型的数组+链表的数据结构。

1. 当HashMap中put元素的时候，程序首先根据key的hashCode()决定Entry的存储位置：如果两个Entry的key的hashCode()相同，那它们的存储位置相同。然后通过key.equals()方法判断，如果为true，则新Entry的value将覆盖集合中原有Entry的value，但key不会覆盖。如果是false，那么将新的Entry以头插法插入对应位置的链表中。
2. 当HashMap中get元素的时候，首先计算key的hashCode，找到数组中对应位置的某一元素，然后通过key的equals()方法在对应位置的链表中找到需要的元素。

```
    @Override public V put(K key, V value) {
        if (key == null) {
            return putValueForNullKey(value);
        }

        int hash = secondaryHash(key);
        HashMapEntry<K, V>[] tab = table;
        int index = hash & (tab.length - 1);
        for (HashMapEntry<K, V> e = tab[index]; e != null; e = e.next) {
            if (e.hash == hash && key.equals(e.key)) {
                preModify(e);
                V oldValue = e.value;
                e.value = value;
                return oldValue;
            }
        }

        // No entry for (non-null) key is present; create one
        modCount++;
        if (size++ > threshold) {
            tab = doubleCapacity();
            index = hash & (tab.length - 1);
        } 
        addNewEntry(key, value, hash, index);
        return null;
    }
    public V get(Object key) {
        if (key == null) {
            HashMapEntry<K, V> e = entryForNullKey;
            return e == null ? null : e.value;
        }

        int hash = key.hashCode();
        hash ^= (hash >>> 20) ^ (hash >>> 12);
        hash ^= (hash >>> 7) ^ (hash >>> 4);

        HashMapEntry<K, V>[] tab = table;
        for (HashMapEntry<K, V> e = tab[hash & (tab.length - 1)];
                e != null; e = e.next) {
            K eKey = e.key;
            if (eKey == key || (e.hash == hash && key.equals(eKey))) {
                return e.value;
            }
        }
        return null;
    }
```

**扩容**：

HashMap默认的负载因子是0.75，如果HashMap的key-map个数超过了极限容量（容量与加载因子的乘积），那么就需要扩容，扩容代码写的也是相当精巧，会把链表上的元素均匀分布到新的数组中。

JDK 1.7 需要rehash，然后旧链表迁移新链表时，链表元素会倒置，而JDK 1.8 则不会。

```
    final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            // 原索引
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            // 原索引 + oldCap
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
```

**Hash冲突解决办法**

- 开放定址法（线性探测再散列、二次探测再散列、伪随机探测再散列）
- 再哈希法
- 链地址法，建立一个公共溢出区。（HashMap就是采用的链地址法）

**HashMap 和 HashTable 区别**

- HashMap的线程非安全的，HashTable是线程安全。
- HashMap的key值可以为null值，但是hashTable不可。
- HashMap基于AbstractMap，HashTable基于Dictionary类。

## 未完待续...

## 参考

- [美团技术博客](http://tech.meituan.com/java-hashmap.html)