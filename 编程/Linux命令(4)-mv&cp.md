### mv

- **功能**: 将源文件重命名为目标文件，或将源文件移动至指定目录。

- **格式**: 
```
用法：mv [选项]... [-T] 源文件 目标文件
　或：mv [选项]... 源文件... 目录
　或：mv [选项]... -t 目录 源文件...
```

- **参数**
```
长选项必须使用的参数对于短选项时也是必需使用的。
      --backup[=CONTROL]       为每个已存在的目标文件创建备份
  -b                           类似--backup 但不接受参数
  -f, --force                  覆盖前不询问
  -i, --interactive            覆盖前询问
  -n, --no-clobber             不覆盖已存在文件.如果您指定了-i、-f、-n 中的多个，仅最后一个生效。
      --strip-trailing-slashes  去掉每个源文件参数尾部的斜线
  -S, --suffix=SUFFIX           替换常用的备份文件后缀
  -t, --target-directory=DIRECTORY      将所有参数指定的源文件或目录移动至 指定目录
  -T, --no-target-directory     将目标文件视作普通文件处理
  -u, --update                  只在源文件文件比目标文件新，或目标文件
                                不存在时才进行移动
  -v, --verbose         详细显示进行的步骤
      --help            显示此帮助信息并退出
      --version         显示版本信息并退出
备份文件的后缀为"~"，除非以--suffix 选项或是SIMPLE_BACKUP_SUFFIX
环境变量指定。版本控制的方式可通过--backup 选项或VERSION_CONTROL 环境
变量来选择。以下是可用的变量值：
  none, off       不进行备份(即使使用了--backup 选项)
  numbered, t     备份文件加上数字进行排序
  existing, nil   若有数字的备份文件已经存在则使用数字，否则使用普通方式备份
  simple, never   永远使用普通方式备份
```

### cp

- **功能**: 将源文件复制至目标文件，或将多个源文件复制至目标目录。

- **格式**: 
```
用法：cp [选项]... [-T] 源文件 目标文件
　或：cp [选项]... 源文件... 目录
　或：cp [选项]... -t 目录 源文件...
```

- **参数**:
```
长选项必须使用的参数对于短选项时也是必需使用的。
  -a, --archive			等于-dR --preserve=all
      --backup[=CONTROL	为每个已存在的目标文件创建备份
  -b				    类似--backup 但不接受参数
      --copy-contents	在递归处理是复制特殊文件内容
  -d				    等于--no-dereference --preserve=links
  -f, --force			如果目标文件无法打开则将其移除并重试(当 -n 选项存在时则不需再选此项)
  -i, --interactive		覆盖前询问(使前面的 -n 选项失效)
  -H				    跟随源文件中的命令行符号链接
  -l, --link			链接文件而不复制
  -L, --dereference		总是跟随符号链接
  -n, --no-clobber		不要覆盖已存在的文件(使前面的 -i 选项失效)
  -P, --no-dereference	不跟随源文件中的符号链接
  -p				    等于--preserve=模式,所有权,时间戳
      --preserve[=属性列表	保持指定的属性(默认：模式,所有权,时间戳)，如果可能保持附加属性：环境、链接、xattr 等
  -c                           same as --preserve=context
      --sno-preserve=属性列表	不保留指定的文件属性
      --parents			    复制前在目标目录创建来源文件路径中的所有目录
  -R, -r, --recursive		递归复制目录及其子目录内的所有内容
      --reflink[=WHEN]		控制克隆/CoW 副本。请查看下面的内如。
      --remove-destination	尝试打开目标文件前先删除已存在的目的地文件 (相对于 --force 选项)
      --sparse=WHEN		    控制创建稀疏文件的方式
      --strip-trailing-slashes	删除参数中所有源文件/目录末端的斜杠
  -s, --symbolic-link		只创建符号链接而不复制文件
  -S, --suffix=后缀		    自行指定备份文件的后缀
  -t,  --target-directory=目录	将所有参数指定的源文件/目录复制至目标目录
  -T, --no-target-directory	将目标目录视作普通文件
  -u, --update                 copy only when the SOURCE file is newerthan the destination file or when thedestination file is missing
  -v, --verbose                explain what is being done
  -x, --one-file-system        stay on this file system
  -Z, --context=CONTEXT        set security context of copy to CONTEXT
      --help		显示此帮助信息并退出
      --version		显示版本信息并退出
```