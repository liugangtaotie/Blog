## Label

众所周知，C语言有一个goto关键字，可以用来跳出多重循环或者某个运行点。Java虽然保留了goto关键字，但是没有实现其功能，为了方便灵活的使用循环，我们可以使用标签来完成这些功能。

## break label

**执行结果是执行 label 的下一步**

### 多重循环

```
    public static void main(String[] args) {
        // 跳出多重循环体
        label:
        for (int i = 0; i < 10; i++) {
            System.out.println("i >>> " + i);
            for (int j = 0; j < 10; j++) {
                System.out.print(j + " ");
                if (j == 6) {
                    break label;
                }
            }
        }
    }
```

结果是

```
i >>> 0
0 1 2 3 4 5 6 
```

### 类似GoTo

```
    public static void main(String[] args) {
        // break label 此时相当于 goto label 的下一步
        for (int i = 0; i < 10; i++) {
            System.out.println("i>>>" + i + " start");

            firstLabel:
            {
                System.out.println("firstLabel start");
                secondLabel:
                {
                    System.out.println("secondLabel start");
                    thirdLabel:
                    {
                        System.out.println("thirdLabel start");
                        if (i == 2) {
                            break firstLabel;
                        } else if (i == 4) {
                            break secondLabel;
                        } else if (i == 6) {
                            break thirdLabel;
                        }
                        System.out.println("thirdLabel end");
                    }
                    System.out.println("secondLabel end");
                }
                System.out.println("firstLabel end");
            }
            System.out.println("i>>>" + i + " end");
            System.out.println("----------------------");
        }
    }
```

结果如下：注意2、4、6的时候

```
i>>>0 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>0 end
----------------------
i>>>1 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>1 end
----------------------
i>>>2 start
firstLabel start
secondLabel start
thirdLabel start
i>>>2 end
----------------------
i>>>3 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>3 end
----------------------
i>>>4 start
firstLabel start
secondLabel start
thirdLabel start
firstLabel end
i>>>4 end
----------------------
i>>>5 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>5 end
----------------------
i>>>6 start
firstLabel start
secondLabel start
thirdLabel start
secondLabel end
firstLabel end
i>>>6 end
----------------------
i>>>7 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>7 end
----------------------
i>>>8 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>8 end
----------------------
i>>>9 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>9 end
----------------------
```

## continue label

**注意：执行结果是 continue label 下最外层的循环**

### 单层循环

```
    public static void main(String[] args) {
        labelForContinue:
        for (int k = 0; k < 5; k++) {
            if (k == 2) {
                continue labelForContinue;
            }
            System.out.print(k + " ");
        }
    }
    // 结果 0 1 3 4
```

### 两层循环

九九乘法表

```
    public static void main(String[] args) {
        outer:
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j < 10; j++) {
                if (j > i) {
                    System.out.println();
                    continue outer;
                }
                System.out.print(" " + (i * j));
            }
        }
        System.out.println("\n----分割线-----等同如下代码------");
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j <= i; j++) {
                System.out.print(" " + i * j);
            }
            System.out.println();
        }
    }
```

结果是

```
 0
 0 1
 0 2 4
 0 3 6 9
 0 4 8 12 16
 0 5 10 15 20 25
 0 6 12 18 24 30 36
 0 7 14 21 28 35 42 49
 0 8 16 24 32 40 48 56 64
 0 9 18 27 36 45 54 63 72 81
----分割线-----等同如下代码------
 0
 0 1
 0 2 4
 0 3 6 9
 0 4 8 12 16
 0 5 10 15 20 25
 0 6 12 18 24 30 36
 0 7 14 21 28 35 42 49
 0 8 16 24 32 40 48 56 64
 0 9 18 27 36 45 54 63 72 81
```
