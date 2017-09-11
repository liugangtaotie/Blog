## Number

作为左右数字的抽象基类，Number非常简单

```
public abstract int intValue();
public abstract long longValue();
public abstract float floatValue();
public abstract double doubleValue();
public byte byteValue() { return (byte)intValue(); }
public short shortValue() { return (short)intValue(); }
```

## Short

略过，详见 Integer

## Integer

```
public static String toString(int i, int radix)
public static String toUnsignedString(int i, int radix)
public static String toHexString(int i)    // 以十六进制（基数 16）无符号整数形式返回一个整数参数的字符串表示形式
public static String toOctalString(int i)  // 以八进制（基数 8）无符号整数形式返回一个整数参数的字符串表示形式
public static String toBinaryString(int i) // 以二进制（基数 2）无符号整数形式返回一个整数参数的字符串表示形式
public static String toString(int i)
public static String toUnsignedString(int i)
public static int parseInt(String s, int radix)
public static int parseInt(String s) throws NumberFormatException
public static int parseUnsignedInt(String s, int radix)
public static int parseUnsignedInt(String s) throws NumberFormatException
public static Integer valueOf(String s, int radix) throws NumberFormatException
public static Integer valueOf(String s) throws NumberFormatException
public static Integer valueOf(int i)
public static int hashCode(int value)
public static Integer getInteger(String nm)
public static Integer getInteger(String nm, int val)
public static Integer getInteger(String nm, Integer val)
public static Integer decode(String nm) throws NumberFormatException // 将 String 解码为 Integer。接受通过以下语法给出的十进制、十六进制和八进制数字
public static int compare(int x, int y)
public static int compareUnsigned(int x, int y)
public static long toUnsignedLong(int x)
public static int divideUnsigned(int dividend, int divisor)
public static int remainderUnsigned(int dividend, int divisor)
public static int highestOneBit(int i) // 返回 具有至多单个 1 位的 int 值，在指定的 int 值中最高位（最左边）的 1 位的位置。
public static int lowestOneBit(int i)  // 返回 具有至多单个 1 位的 int 值，在指定的 int 值中最低位（最右边）的 1 位的位置。
public static int numberOfLeadingZeros(int i)
public static int numberOfTrailingZeros(int i)
public static int bitCount(int i) // 返回指定 int 值的二进制补码表示形式的 1 位的数量
public static int rotateLeft(int i, int distance)
public static int rotateRight(int i, int distance)
public static int reverse(int i)  // 返回通过反转指定 int 值的二进制补码表示形式中位的顺序而获得的值
public static int signum(int i)   // 返回指定 int 值的符号函数
public static int reverseBytes(int i) // 返回通过反转指定 int 值的二进制补码表示形式中字节的顺序而获得的值。
public static int sum(int a, int b)
public static int max(int a, int b)
public static int min(int a, int b)
```

## BigInteger

下面主要列举一下跟Integer不太一样的方法

```
public static BigInteger probablePrime(int bitLength, Random rnd) // 返回有可能是素数的、具有指定长度的正 BigInteger
public BigInteger(int numBits, Random rnd) {...}    // 随机数
public boolean isProbablePrime(int certainty) {...} // 是否是素数
public BigInteger nextProbablePrime() {...}         // 返回大于此 BigInteger 的可能为素数的第一个整数
public BigInteger add(BigInteger val)
public BigInteger subtract(BigInteger val) // 返回其值为 (this - val) 的 BigInteger
public BigInteger multiply(BigInteger val) // 返回其值为 (this * val) 的 BigInteger
public BigInteger divide(BigInteger val)   // 返回其值为 (this / val) 的 BigInteger
public BigInteger[] divideAndRemainder(BigInteger val) // 返回包含 (this / val) 后跟 (this % val) 的两个 BigInteger 的数组
public BigInteger remainder(BigInteger val) // 返回其值为 (this % val) 的 BigInteger
public BigInteger pow(int exponent)    // 返回 (this exponent 次方) 的 BigInteger
public BigInteger gcd(BigInteger val)  // 返回 abs(this) 和 abs(val) 的最大公约数
public BigInteger abs()    // 返回此 BigInteger 的绝对值
public BigInteger negate() // 返回 (-this) 的 BigInteger
public int signum()
public BigInteger mod(BigInteger m)
public BigInteger modPow(BigInteger exponent, BigInteger m)
public BigInteger modInverse(BigInteger m)
public BigInteger shiftLeft(int n)  // 返回其值为 (this << n) 的 BigInteger
public BigInteger shiftRight(int n) // 返回其值为 (this >> n) 的 BigInteger
public BigInteger and(BigInteger val) // 与
public BigInteger or(BigInteger val)  // 或
public BigInteger xor(BigInteger val) // 返回 (this ^ val) 的 BigInteger
public BigInteger not() // 返回为 (~this) 的 BigInteger
public BigInteger andNot(BigInteger val) // 返回 (this & ~val) 的 BigInteger（ 此方法等效于 and(val.not()) ）
public boolean testBit(int n)
public BigInteger setBit(int n)   // 返回其值与设置了指定位的此 BigInteger 等效的 BigInteger（计算 (this | (1<<n))。）
public BigInteger clearBit(int n) // 返回其值与清除了指定位的此 BigInteger 等效的 BigInteger（计算 (this & ~(1<<n))。）
public BigInteger flipBit(int n)  // 返回其值与对此 BigInteger 进行指定位翻转后的值等效的 BigInteger。（计算 (this ^ (1<<n))。）
public int getLowestSetBit()      // 返回此 BigInteger 最右端（最低位）1 比特的索引（即从此字节的右端开始到本字节中最右端 1 比特之间的 0 比特的位数）。如果此 BigInteger 不包含一位，则返回 -1。
public int bitLength()
public int bitCount()
```

## AtomicInteger

可以用原子方式更新的 int 值

```
public AtomicInteger(int initialValue)
public AtomicInteger()
public final int get()
public final void set(int newValue)
public final void lazySet(int newValue)
public final int getAndSet(int newValue) // 以原子方式设置为给定值，并返回旧值
public final boolean compareAndSet(int expect, int update)
public final boolean weakCompareAndSet(int expect, int update)
public final int getAndIncrement() // 以原子方式将当前值加 1
public final int incrementAndGet() // 以原子方式将当前值加 1
public final int getAndDecrement() // 以原子方式将当前值减 1
public final int decrementAndGet() // 以原子方式将当前值减 1
public final int getAndAdd(int delta) // 以原子方式将给定值与当前值相加
public final int addAndGet(int delta) // 以原子方式将给定值与当前值相加
public final int getAndUpdate(IntUnaryOperator updateFunction)
public final int updateAndGet(IntUnaryOperator updateFunction)
public final int getAndAccumulate(int x,
public final int accumulateAndGet(int x,
```

## Long

长度更加长的 Integer

## AtomicLong

可以以院子方式更新的 long 值

## Double 

```
public static String toHexString(double d) // 返回 double 参数的十六进制字符串表示形式（具体请看文档）
```

## BigDecimal

不可变的、任意精度的有符号十进制数

BigDecimal 类提供以下操作：算术、标度操作、舍入、比较、哈希算法和格式转。具体请看文档

## Float

精度不如Double

## 参考

- http://tool.oschina.net/apidocs/apidoc?api=jdk-zh
- http://blog.csdn.net/zq602316498/article/details/41148063
