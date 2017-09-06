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

```

**内存泄露**

```

```


## CharSequence

```
int length();
char charAt(int index);
CharSequence subSequence(int start, int end);
public String toString();
public default IntStream chars(){...}
public default IntStream codePoints(){...}
```

## StringBuilder

## StringBuffer

## Character

## 参考

- http://www.jianshu.com/p/799c4459b808