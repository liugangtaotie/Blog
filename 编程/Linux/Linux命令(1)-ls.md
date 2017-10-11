### 功能

简写列出目标目录中所有的子目录和文件

### 格式

```
ls [选项] [目录名]
```

### 参数

```
  -a, --all                     不隐藏任何以. 开始的项目
  -A, --almost-all              列出除. 及.. 以外的任何项目
      --author                  与-l 同时使用时列出每个文件的作者
  -b, --escape                  以八进制溢出序列表示不可打印的字符
      --block-size=SIZE         SIZE 可以是一个可选的整数，后面跟着以下单位中的一个：
                                KB 1000，K 1024，MB 1000*1000，M 1024*1024，还有 G、T、P、E、Z、Y。
  -B, --ignore-backups          不要列出以 ~ 结尾的隐含项
  -c                            配合 -lt：根据 ctime 排序及显示 ctime (文件状态最后更改的时间)
                                配合 -l：显示 ctime 根据名称排序及
                                否则: 根据 ctime 排序 最新的在前面
  -C                            每栏由上至下列出项目
      --color[=WHEN]            控制是否使用色彩分辨文件。WHEN 可以是
                                "never"(默认)、"always"或"auto"其中之一
  -d, --directory               当遇到目录时列出目录本身而非目录内的文件
  -D, --dired                   产生适合Emacs 的dired 模式使用的结果
  -f                            不进行排序，-aU 选项生效，-lst 选项失效
  -F, --classify                加上文件类型的指示符号(*/=@| 其中一个)
      --format=关键字            交错-x，逗号分隔-m，水平-x，长-l，
                                单栏-1，详细-l，垂直-C
      --full-time               即-l --time-style=full-iso
  -g                            类似-l，但不列出所有者
      --group-directories-first 在文件前分组目录。此选项可与--sort 一起使用，
                                但是一旦使用--sort=none (-U)将禁用分组
  -G, --no-group                以一个长列表的形式，不输出组名
  -h, --human-readable          与-l 一起，以易于阅读的格式输出文件大小
                                (例如 1K 234M 2G)
      --si                      同上面类似，但是使用1000 为基底而非1024
  -H, --dereference-command-line
                                跟随命令行列出的符号链接
      --dereference-command-line-symlink-to-dir
                                跟随命令行列出的目录的符号链接
      --hide=PATTERN            隐藏符合PATTERN 模式的项目
                                (-a 或 -A 将覆盖此选项)
      --indicator-style=方式    指定在每个项目名称后加上指示符号方式：
                                none (默认)，classify (-F)，file-type (-p)
  -i, --inode                   显示每个文件的inode 号
  -I, --ignore=PATTERN          不显示任何符合指定shell PATTERN 的项目
  -k                            即--block-size=1K
  -l                            使用较长格式列出信息
  -L, --dereference             当显示符号链接的文件信息时，显示符号链接所指示
                                的对象而并非符号链接本身的信息
  -m                            所有项目以逗号分隔，并填满整行行宽
  -n, --numeric-uid-gid         类似 -l，但列出UID 及GID 号
  -N, --literal                 输出未经处理的项目名称 (如不特别处理控制字符)
  -o                            类似 -l，但不列出有关组的信息
  -p,  --indicator-style=slash  对目录加上表示符号"/"
  -q, --hide-control-chars      以"?"字符代替无法打印的字符
      --show-control-chars      直接显示无法打印的字符 (这是默认方式，除非调用
                                的程序名称是"ls"而且是在终端输出结果)
  -Q, --quote-name              将条目名称括上双引号
      --quoting-style=方式      使用指定的quoting 方式显示条目的名称：
                                literal、locale、shell、shell-always、c、escape
  -r, --reverse                 逆序排列
  -R, --recursive               递归显示子目录
  -s, --size                    以块数形式显示每个文件分配的尺寸
  -S                            根据文件大小排序
      --sort=WORD               以下是可选用的WORD 和它们代表的相应选项：
                                extension -X       status   -c
                                none      -U       time     -t
                                size      -S       atime    -u
                                time      -t       access   -u
                                version   -v       use      -u
      --time=WORD               和-l 同时使用时显示WORD 所代表的时间而非修改时
                                间：atime、access、use、ctime 或status；加上
                                --sort=time 选项时会以指定时间作为排序关键字
      --time-style=STYLE        和-l 同时使用时根据STYLE 代表的格式显示时间：
                                full-iso、iso、locale、posix-iso、+FORMAT。
                                FORMAT 即是"date"所用的时间格式；如果FORMAT
                                是FORMAT1<换行>FORMAT2，FORMAT1 适用于较旧
                                的文件而FORMAT2 适用于较新的文件；如果STYLE
                                以"posix-"开头，则STYLE 仅在POSIX 语系之外
                                生效。
  -t                            根据修改时间排序
  -T, --tabsize=宽度    指定制表符(Tab)的宽度，而非8 个字符
  -t                         sort by modification time, newest first
  -T, --tabsize=COLS         assume tab stops at each COLS instead of 8
  -u                    同-lt 一起使用：按照访问时间排序并显示
                        同-l一起使用：显示访问时间并按文件名排序
                        其他：按照访问时间排序
  -U                    不进行排序；按照目录顺序列出项目
  -v                    在文本中进行数字(版本)的自然排序
  -w, --width=COLS      自行指定萤幕宽度而不使用目前的数值
  -x                    逐行列出项目而不是逐栏列出
  -X                    根据扩展名排序
  -1                    每行只列出一个文件
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出
      
使用色彩来区分文件类型的功能已被禁用，默认设置和 --color=never 同时禁用了它。
使用 --color=auto 选项，ls 只在标准输出被连至终端时才生成颜色代码。
LS_COLORS 环境变量可改变此设置，可使用 dircolors 命令来设置。

退出状态：
 0  正常
 1  一般问题 (例如：无法访问子文件夹)
 2  严重问题 (例如：无法使用命令行参数)
```

### 示例

**示例一**
```
[root@bogon /]# ls -al
总用量 110
dr-xr-xr-x.  25 root root  4096 3月  24 12:51 .
dr-xr-xr-x.  25 root root  4096 3月  24 12:51 ..
-rw-r--r--.   1 root root     0 3月  24 12:51 .autofsck
dr-xr-xr-x.   2 root root  4096 3月  29 11:28 bin
dr-xr-xr-x.   5 root root  1024 3月  24 12:49 boot
drwxr-xr-x.  10 root root  4096 3月  24 12:51 cgroup
drwxr-xr-x.  21 root root  3980 3月  29 13:02 dev
drwxr-xr-x. 117 root root 12288 3月  29 13:02 etc
drwxr-xr-x.   4 root root  4096 3月  24 12:49 home
dr-xr-xr-x.  10 root root  4096 3月  29 11:28 lib
dr-xr-xr-x.  10 root root 12288 3月  29 11:28 lib64
drwx------.   2 root root 16384 3月  24 12:43 lost+found
drwxr-xr-x.   3 root root  4096 3月  24 12:50 media
drwxr-xr-x.   2 root root     0 3月  24 12:51 misc
drwxr-xr-x.   2 root root  4096 9月  23 2011 mnt
drwxr-xr-x.   2 root root     0 3月  24 12:51 net
drwxr-xr-x.   3 root root  4096 3月  24 12:48 opt
dr-xr-xr-x. 131 root root     0 3月  24 12:51 proc
dr-xr-x---.   3 root root  4096 3月  29 12:34 root
dr-xr-xr-x.   2 root root 12288 3月  29 11:28 sbin
drwxr-xr-x.   7 root root     0 3月  24 12:51 selinux
drwxr-xr-x.   2 root root  4096 9月  23 2011 srv
drwxr-xr-x.  13 root root     0 3月  24 12:51 sys
drwxrwxrwt.   5 root root  4096 3月  29 13:02 tmp
drwxr-xr-x.  13 root root  4096 3月  24 12:43 usr
drwxr-xr-x.  22 root root  4096 3月  24 12:47 var
```

**示例二**
```
[root@bogon ~]# ls -al --time-style=long-iso
总用量 128
dr-xr-x---.  4 root root  4096 2017-04-11 10:33 .
dr-xr-xr-x. 25 root root  4096 2017-03-24 12:51 ..
-rw-------.  1 root root  2663 2017-03-24 12:49 anaconda-ks.cfg
-rw-------.  1 root root  3856 2017-04-11 01:13 .bash_history
-rw-r--r--.  1 root root    18 2009-05-20 18:45 .bash_logout
-rw-r--r--.  1 root root   176 2009-05-20 18:45 .bash_profile
-rw-r--r--.  1 root root   176 2004-09-23 11:59 .bashrc
-rw-r--r--.  1 root root   100 2004-09-23 11:59 .cshrc
-rw-r--r--.  1 root root 53675 2017-03-24 12:49 install.log
-rw-r--r--.  1 root root 12179 2017-03-24 12:47 install.log.syslog
-rw-------.  1 root root    43 2017-04-08 15:09 .lesshst
-rw-------.  1 root root    32 2017-03-29 12:34 .mysql_history
drwxr-----.  3 root root  4096 2017-03-29 11:30 .pki
-rw-r--r--.  1 root root   129 2004-12-04 05:42 .tcshrc
drwxr-xr-x.  2 root root  4096 2017-04-11 10:33 test
-rw-------.  1 root root  3363 2017-04-11 10:33 .viminfo
```
