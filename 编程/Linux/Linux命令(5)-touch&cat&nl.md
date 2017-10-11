### touch

- **功能**: 将每个文件的访问时间和修改时间改为当前时间。不存在的文件将会被创建为空文件，除非使用`-c`或`-h`选项。

- **格式**: 
```
touch [选项]... 文件...
```

- **参数**
```
长选项必须使用的参数对于短选项时也是必需使用的。
  -a                    只更改访问时间
  -c, --no-create       不创建任何文件
  -d, --date=字符串     使用指定字符串表示时间替代当前时间
  -f                    (忽略)
  -h, --no-dereference  会影响符号链接本身，替代符号链接所指示的目的地(当系统支持更改符号链接的所有者时，此选项才有用)
  -m                    只更改修改时间
  -r, --reference=文件  使用指定文件的时间属性替代当前时间
  -t STAMP              使用[[CC]YY]MMDDhhmm[.ss] 格式的时间替代当前时间
  --time=WORD           使用WORD 指定的时间：access、atime、use 都等于-a选项的效果，而modify、mtime 等于-m 选项的效果
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出
请注意，-d 和-t 选项可接受不同的时间/日期格式。
```

- **示例**
```
touch test.php                # 将test.php的档案时间改为，当前时间，文件不存在建之
touch -c -t 05061803 test.php # 将档案时间改为,5月6日18点3分
touch -r abc.php test.php     # 将test.php档案改成跟abc.php一样
touch -d "2 days ago" test.php# 将test.php日期修改为2天以前
```

### cat

- **功能**: 将[文件]或标准输入组合输出到标准输出。
 + 显示整个文件内容
 + 键盘创建一个文件 `cat > filename`
 + 将多个文件合并 `cat file1 file2 > file`

- **格式**: 
```
cat [选项]... [文件]...
```

- **参数**
```
  -A, --show-all           等于-vET
  -b, --number-nonblank    对非空输出行编号
  -e                       等于-vE
  -E, --show-ends          在每行结束处显示"$"
  -n, --number             对输出的所有行编号
  -s, --squeeze-blank      不输出多行空行
  -t                       与-vT 等价
  -T, --show-tabs          将跳格字符显示为^I
  -u                       (被忽略)
  -v, --show-nonprinting   使用^ 和M- 引用，除了LFD和 TAB 之外
      --help		显示此帮助信息并退出
      --version		显示版本信息并退出
```

- **示例**
```
cat -n test1.txt > test2.txt        # 同下
cat -n test1.txt test2.txt          # 把test1.txt的文件内容加上行号后输入到test2.txt这个文件里
cat -b test1.txt test2.txt test.txt # 把test1.txt和test2.txt的文件内容加上行号（空白行不加）附加到test.txt文件中

```

### nl

- **功能**: 将指定的各个文件添加行号标注后写到标准输出。其默认的结果与`cat -n`类似

- **格式**: 
```
nl [选项]... [文件]...
```

- **参数**
```
长选项必须使用的参数对于短选项时也是必需使用的。
  -b, --body-numbering=样式	    使用指定样式编号文件的正文行目
  -d, --section-delimiter=CC	使用指定的CC分割逻辑页数
  -f, --footer-numbering=样式	使用指定样式编号文件的页脚行目
  -h, --header-numbering=样式	使用指定样式编号文件的页眉行目
  -i, --page-increment=数值	    设置每一行遍历后的自动递增值
  -l, --join-blank-lines=数值	设置数值为多少的若干空行被视作一行
  -n, --number-format=格式	    根据指定格式插入行号
  -p, --no-renumber		        在逻辑页数切换时不将行号值复位
  -s, --number-separator=字符串	可能的话在行号后添加字符串
  -v, --starting-line-number=数字	每个逻辑页上的第一行的行号
  -w, --number-width=数字	    为行号使用指定的栏数
      --help		显示此帮助信息并退出
      --version		显示版本信息并退出
```
