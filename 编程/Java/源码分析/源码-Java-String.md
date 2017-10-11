## CharSequence

字符序列接口，String，AbstractStringBuilder，StringBuffer，StringBuilder都是实现了这个接口。

```
int length();
char charAt(int index);
CharSequence subSequence(int start, int end);
public String toString();
public default IntStream chars(){...}
public default IntStream codePoints(){...}
```

## String

```
public int length(){...}
public boolean isEmpty(){...}
public char charAt(int index){...}
public int codePointAt(int index){...}
public int codePointBefore(int index){...}
public int codePointCount(int beginIndex, int endIndex){...}
public int offsetByCodePoints(int index, int codePointOffset){...}
void getChars(char dst[], int dstBegin){...}
public void getChars(int srcBegin, int srcEnd, char dst[], int dstBegin){...}
public byte[] getBytes(String charsetName){...}
public boolean contentEquals(StringBuffer sb){...}
public int compareTo(String anotherString){...}
public boolean regionMatches(int toffset, String other, int ooffset, int len){...
public boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len){...}
public boolean startsWith(String prefix, int toffset)
public boolean startsWith(String prefix)
public boolean endsWith(String suffix)
public int hashCode(){...}
public int indexOf(int ch) {...}
public int indexOf(int ch, int fromIndex) {...}
public int indexOf(String str, int fromIndex) {...}
public int lastIndexOf(int ch) {...}
public int lastIndexOf(int ch, int fromIndex) {...}
public int lastIndexOf(String str) {...}
public int lastIndexOf(String str, int fromIndex) {...}
public String substring(int beginIndex) {...}
public String substring(int beginIndex, int endIndex) {...}
public CharSequence subSequence(int beginIndex, int endIndex) {...}
public String concat(String str) {...}
public String replace(char oldChar, char newChar) {...}
public boolean matches(String regex) {...}
public boolean contains(CharSequence s) {...}
public String replaceFirst(String regex, String replacement) {...}
public String replaceAll(String regex, String replacement) {...}
public String[] split(String regex, int limit) {...}
public static String join(CharSequence delimiter, CharSequence... elements) {...}
public String toLowerCase(Locale locale) {...}
public String toUpperCase(Locale locale) {...}
public String trim() {...}
public String toString() {...}
public char[] toCharArray() {...}
public static String format(String format, Object... args) {...}
public static String format(Locale l, String format, Object... args) {...}
public static String valueOf(Object obj) {...}
public static String copyValueOf(char data[]) {...}
public static String copyValueOf(char data[], int offset, int count){...} 
public native String intern();
```

**注意**

- trim():去除首尾空格，而非全部空格
- encode():如果没有指定编码方式，那么就会使用系统默认编码，默认是ISO-8859-1或者GBK/GB2312
- equals():被String类重写了equals方法

**内存泄露**

为了防止内存泄露，因此现在Java 7很多方法（substring replace concat）使用生成新的char数组修复这个BUG，虽然损失了性能且浪费空间，但比之前更加健壮。

**字符串相加**

示例如下

```
public class DemoForStringPlus {
    public static void main(String[] args) {
        String a = "aaa";
        String b = "bbb";
        String c = "ccc";
        String d = "ddd" + a + b + c;
        String f = "eee" + "fff";
        System.out.println(d);
        System.out.println(f);
    }
}
```

我们看看字节码

```
Compiled from "DemoForStringPlus.java"
public class DemoForStringPlus {
  public DemoForStringPlus();
    Code:
       0: aload_0
       1: invokespecial #1  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: ldc   #2  // String aaa
       2: astore_1
       3: ldc   #3  // String bbb
       5: astore_2
       6: ldc   #4  // String ccc
       8: astore_3
       9: new   #5  // class java/lang/StringBuilder
      12: dup
      13: invokespecial #6  // Method java/lang/StringBuilder."<init>":()V
      16: ldc   #7  // String ddd
      18: invokevirtual #8  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      21: aload_1
      22: invokevirtual #8  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      25: aload_2
      26: invokevirtual #8  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      29: aload_3
      30: invokevirtual #8  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      33: invokevirtual #9  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
      36: astore4
      38: ldc   #10 // String eeefff
      40: astore5
      42: getstatic     #11 // Field java/lang/System.out:Ljava/io/PrintStream;
      45: aload 4
      47: invokevirtual #12 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      50: getstatic     #11 // Field java/lang/System.out:Ljava/io/PrintStream;
      53: aload 5
      55: invokevirtual #12 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      58: return
}
```

我们发现

1. 字符串变量的加号实际上使用 StringBuilder or StringBuffer 来实现的，如果是循环使用加号添加，那么效率会很低，因为每次都需要新建一个 StringBuilder or StringBuffer 对象。
2. 字符串常量的加号是直接生成的

**intern():** 

如果字符串常量池已经包含一个等于此 String 对象的字符串（注：该对象由 equals(Object) 方法确定），则返回池中的字符串。否则，将此 String 对象添加到池中，并且返回此 String 对象的引用

**加号、双等号、intern()、new String()大比较**

因为String太常用，因此String类设计成了享元模式，每当生成一个新内容的字符串时，他们都被添加到一个共享池中，当第二次再次生成同样内容的字符串实例时，就共享此对象，而不是创建一个新对象，这样的做法仅仅适合于通过=符号进行的初始化

经过上面一系列的讲解，相信下面的代码就能很快理解了。（由于 equals 直接比较的是内容，因此这里我们不再写equals的相关示例）

```
String str1 = "a";
String str2 = "b";
String str3 = "ab";              // str3 指向字符串常量内存地址
String str4 = new String("ab");  // str4 指向对象引用内存地址
String str5 = "a" + "b";         // str5 指向字符串常量内存地址
String str6 = str1 + str2;       // str6 指向的是StringBuilder/StringBuffer的对象引用内存地址

System.out.println("-------------str3/str5----------------");
System.out.println(str3 == str5);                   // true
System.out.println(str3.intern() == str5);          // true
System.out.println(str3 == str5.intern());          // true
System.out.println(str3.intern() == str5.intern()); // true

System.out.println("-------------str3/str6----------------");
System.out.println(str3 == str6);                   // false
System.out.println(str3.intern() == str6);          // false
System.out.println(str3 == str6.intern());          // true
System.out.println(str3.intern() == str6.intern()); // true

System.out.println("-------------str3/str4----------------");
System.out.println(str3 == str4);                   // false
System.out.println(str3.intern() == str4);          // false
System.out.println(str3 == str4.intern());          // true
System.out.println(str3.intern() == str4.intern()); // true

System.out.println("-------------str4/str5----------------");
System.out.println(str4 == str5);                   // false
System.out.println(str4.intern() == str5);          // true
System.out.println(str4 == str5.intern());          // false
System.out.println(str4.intern() == str5.intern()); // true

System.out.println("-------------str4/str6----------------");
System.out.println(str4 == str6);                   // false
System.out.println(str4.intern() == str6);          // false
System.out.println(str4 == str6.intern());          // false
System.out.println(str4.intern() == str6.intern()); // true

System.out.println("-------------str5/str6----------------");
System.out.println(str5 == str6);                   // false
System.out.println(str5.intern() == str6);          // false
System.out.println(str5 == str6.intern());          // true
System.out.println(str5.intern() == str6.intern()); // true
```

- str3 指向字符串常量内存地址
- str4 指向对象引用内存地址
- str5 指向字符串常量内存地址
- str6 指向的是StringBuilder/StringBuffer的对象引用内存地址

## AbstractStringBuilder

抽象类，除了CharSequence里面的方法外，还有如下方法

```
public int capacity() {...}
public void ensureCapacity(int minimumCapacity) {...} 
public void trimToSize() {...}
public void setLength(int newLength) {...} 
public int codePointAt(int index) {...}
public int codePointBefore(int index) {...}
public int codePointCount(int beginIndex, int endIndex) {...} 
public int offsetByCodePoints(int index, int codePointOffset) {...} 
public void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin) {...}
public void setCharAt(int index, char ch) {...}
public AbstractStringBuilder append(Object obj) {...} 
public AbstractStringBuilder delete(int start, int end) {...} 
public AbstractStringBuilder deleteCharAt(int index) {...}
public AbstractStringBuilder replace(int start, int end, String str) {...} 
public CharSequence subSequence(int start, int end) {...}
public String substring(int start, int end) {...}
public AbstractStringBuilder insert(int offset, Object obj) {...}
public int indexOf(String str) {...}
public AbstractStringBuilder reverse() {...}
```

## StringBuilder

AbstractStringBuilder抽象类的具体实现

## StringBuffer

线程安全的StringBuilder

## Character

字符处理类，也是char类型的包装类

**非静态方法如下**

```
public Character(char value) {...}
public char charValue() {...}
```

**静态方法如下**

```
public static Character valueOf(char c) // 有128的缓存
public static int hashCode(char value) {return (int)value;}
public static boolean isValidCodePoint(int codePoint) // 是否合法Unicode码（0 to 10FFFF）
public static boolean isBmpCodePoint(int codePoint) // 判断一个Unicode码值是不是属于基本多语言平面
public static boolean isSupplementaryCodePoint(int codePoint) // 判断一个Unicode码值是否是补充字符
public static boolean isHighSurrogate(char ch) // 判断一个字符是不是高位(开始)代理字符
public static boolean isLowSurrogate(char ch) // 判断一个字符是不是低位(或尾部)代理字符
public static boolean isSurrogate(char ch) // 判断一个字符是不是代理字符
public static boolean isSurrogatePair(char high, char low) 
public static int charCount(int codePoint) // 返回指定Unicode码值的字符个数
public static int toCodePoint(char high, char low) // 转换代理对为Unicode码值
public static int codePointAt(CharSequence seq, int index) // 返回字符数组或字符序列指定位置的Unicode码值
public static int codePointAt(char[] a, int index, int limit) 
public static int codePointBefore(CharSequence seq, int index) // 返回字符数组或字符序列指定位置前的Unicode码值 
public static int codePointBefore(char[] a, int index, int start) 
public static char highSurrogate(int codePoint) // 返回Unicode码值的高位(开始)代理字符
public static char lowSurrogate(int codePoint) // 返回补充字符Unicode码值的结尾代理
public static int toChars(int codePoint, char[] dst, int dstIndex) 
public static char[] toChars(int codePoint) // 转换指定Unicode码值为UTF-16表示，保存到字符数组
public static int codePointCount(CharSequence seq, int beginIndex, int endIndex) 
public static int codePointCount(char[] a, int offset, int count) // 返回字符序列或字符数组的Unicode码值的个数 
public static int offsetByCodePoints(CharSequence seq, int index, int codePointOffset) // 返回字符数组或字符序列中指定Unicode码值位置的索引  
public static boolean isLowerCase(char ch) // 	判断一个字符或Unicode码值是不是小写字符
public static boolean isLowerCase(int codePoint) 
public static boolean isUpperCase(char ch) 
public static boolean isUpperCase(int codePoint) 
public static boolean isTitleCase(char ch) // 	判断字符是不是一个titlecase字符
public static boolean isTitleCase(int codePoint) 
public static boolean isDigit(char ch) 
public static boolean isDigit(int codePoint) 
public static boolean isDefined(char ch) 
public static boolean isDefined(int codePoint) 
public static boolean isLetter(char ch) 
public static boolean isLetter(int codePoint) 
public static boolean isLetterOrDigit(char ch)
public static boolean isLetterOrDigit(int codePoint) 
public static boolean isAlphabetic(int codePoint) // 判断一个Unicode码值是不是字母表
public static boolean isIdeographic(int codePoint) // 判断一个字符是不是一个象形文字
public static boolean isJavaIdentifierStart(char ch)
public static boolean isJavaIdentifierStart(int codePoint)
public static boolean isJavaIdentifierPart(char ch)
public static boolean isJavaIdentifierPart(int codePoint)
public static boolean isUnicodeIdentifierStart(char ch)
public static boolean isUnicodeIdentifierStart(int codePoint)
public static boolean isUnicodeIdentifierPart(char ch)
public static boolean isUnicodeIdentifierPart(int codePoint)
public static boolean isIdentifierIgnorable(char ch)
public static boolean isIdentifierIgnorable(int codePoint)
public static char toLowerCase(char ch)
public static int toLowerCase(int codePoint)
public static char toUpperCase(char ch)
public static int toUpperCase(int codePoint)
public static char toTitleCase(char ch)
public static int toTitleCase(int codePoint)
public static int digit(char ch, int radix) // 返回指定字符或Unicode码值指定进制的数字值
public static int digit(int codePoint, int radix)
public static int getNumericValue(char ch) // 返回字符的数字值
public static int getNumericValue(int codePoint)
public static boolean isSpace(char ch)
public static boolean isSpaceChar(char ch) // 判断一个字符或Unicode码值是不是一个Unicode空白
public static boolean isSpaceChar(int codePoint)
public static boolean isWhitespace(char ch) // 	判断字符或Unicode码值是不是一个空格
public static boolean isWhitespace(int codePoint)
public static boolean isISOControl(char ch)
public static boolean isISOControl(int codePoint)
public static int getType(char ch) // 返回表示字符所属分类的值
public static int getType(int codePoint)
public static char forDigit(int digit, int radix) // 返回指定数字的指定进制的字符表示
public static byte getDirectionality(char ch) // 返回指定字符或Unicode码值的Unicode方向属性
public static byte getDirectionality(int codePoint)
public static boolean isMirrored(char ch) // 判断一个字符或Unicode码值是否有配对的字符
public static boolean isMirrored(int codePoint)
public static int compare(char x, char y) // 比较两个字符
public static char reverseBytes(char ch) // 返回指定字符的相反顺序字节组成的字符
public static String getName(int codePoint)
```

## CharBuffer

抽象类，char缓冲类，字符缓冲区

## Segment

表示文本片段的字符数组的 segment。尽管能够直接访问数组，也应将其视为不可变的。此实现提供了对文本片段的快速访问，而且不存在来回复制字符的开销。它实际上是一个未受保护的 String。

## 参考

- Java源码
- http://tool.oschina.net/apidocs/apidoc?api=jdk-zh