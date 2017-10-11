### Label

众所周知，C语言有一个goto关键字，可以用来跳出多重循环或者某个运行点。Java虽然保留了goto关键字，但是没有实现其功能，为了方便灵活的使用循环，我们可以使用标签来完成这些功能。

```
break label    : 执行 label 的下一步（ break  不执行 label 的代码）
continue label : 执行 label 的下一步（ continue 执行 label 的代码）
```

### break

```
    public static void main(String[] args) {
        System.out.println("\n------------- break 跳出 label 层:单层循环 -------------\n");
        labelFor:
        for (int i = 0; i < 10; i++) {
            System.out.print(i + " ");
            if (i == 2) {
                break labelFor;
            }
        }
        System.out.println("\n------------- break 跳出 label 层:多重循环 -------------\n");
        labelForFor:
        for (int i = 0; i < 10; i++) {
            System.out.println("i >>> " + i);
            for (int j = 0; j < 10; j++) {
                System.out.print(j + " ");
                if (j == 2) {
                    break labelForFor;
                }
            }
        }
        System.out.println("\n------------- break 跳出 label 层:执行下一步 -------------\n");
        for (int i = 0; i < 5; i++) {
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
                        if (i == 1) {
                            break firstLabel;
                        } else if (i == 2) {
                            break secondLabel;
                        } else if (i == 3) {
                            break thirdLabel;
                        }
                        System.out.println("thirdLabel end");
                    }
                    System.out.println("secondLabel end");
                }
                System.out.println("firstLabel end");
            }
            System.out.println("i>>>" + i + " end");
            System.out.println("------------------");
        }
    }
```

执行结果如下

```
------------- break 跳出 label 层:单层循环 -------------
0 1 2 
------------- break 跳出 label 层:多重循环 -------------
i >>> 0
0 1 2 
------------- break 跳出 label 层:执行下一步 -------------
i>>>0 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>0 end
------------------
i>>>1 start
firstLabel start
secondLabel start
thirdLabel start
i>>>1 end
------------------
i>>>2 start
firstLabel start
secondLabel start
thirdLabel start
firstLabel end
i>>>2 end
------------------
i>>>3 start
firstLabel start
secondLabel start
thirdLabel start
secondLabel end
firstLabel end
i>>>3 end
------------------
i>>>4 start
firstLabel start
secondLabel start
thirdLabel start
thirdLabel end
secondLabel end
firstLabel end
i>>>4 end
------------------
```

### continue

```
    public static void main(String[] args) {
        System.out.println("\n------------- continue 执行 label 层:单层循环 -------------\n");
        labelForContinue:
        for (int k = 0; k < 5; k++) {
            if (k > 2) {
                continue labelForContinue;
            }
            System.out.print(k + " ");
        }
        System.out.println("\n------------- continue 执行 label 层:多层循环 -------------\n");
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
        System.out.println("\n------------- 上面九九乘法表简写 -------------\n");
        for (int i = 0; i < 10; i++) {
            for (int j = 0; j <= i; j++) {
                System.out.print(" " + i * j);
            }
            System.out.println();
        }
    }
```

执行结果如下

```
------------- continue 执行 label 层:单层循环 -------------
0 1 2 
------------- continue 执行 label 层:多层循环 -------------
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
------------- 上面九九乘法表简写 -------------
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