### MAC配置整理

- 树形显示当前文件夹
```
> vim .bash_profile
> 添加 alias tree="find . -print | sed -e 's;[^/]*/;|---;g;s;---|;   |;g'"
> source .bash_profile
```

- 显示隐藏文件
```
> defaults write com.apple.finder AppleShowAllFiles TRUE
> killall Finder
```

- 配置Android环境变量
```
1.创建:touch ~/.bash_profile
2.编辑:open ~/.bash_profile
3.写入:export PATH=${PATH}:your_tools_path:your_platform-tools_path 【your_tools_path 和 your_platform-tools_path 需要替换为自己的目录】
4.执行:source ~/.bash_profile
5.验证：输入adb回车是否显示
```

- 命令行失效
```
export PATH="/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/X11/bin
```

- Apache
```
重启apache：sudo /usr/sbin/apachectl restart
关闭apache：sudo /usr/sbin/apachectl stop
开启apache：sudo /usr/sbin/apachectl start
```

- Mac 终端变色
```
1.touch ~/.bash_profile
2.open ~/.bash_profile
3.插入
export CLICOLOR=1
export LSCOLORS=ExFxCxDxBxegedabagacad
export TERM=xterm-color
export TERM="xterm-color"
PS1='\[\e[0;33m\]\u\[\e[0m\]@\[\e[0;32m\]\h\[\e[0m\]:\[\e[0;34m\]\w\[\e[0m\]\$ '
alias ls='ls -vG'
alias ll='ls -al'
alias la='ls -a'
alias vi='vim'
4.source ~/.bash_profile
```

- 端口被占用
```
查找端口占用的进程ID : sudo lsof -i :1099
java    6395 wangyuchao   24u  IPv6 0x1c374b36c71b3eb7      0t0  TCP *:rmiregistry (LISTEN)
杀死进程 : sudo kill -9 6395
```

- WebStorm 局域网手机内浏览
```
preferences -> debugger -> 端口号 + can accept extral
```

- Nginx 命令
```
sudo nginx
sudo nginx -s reload
sudo nginx -s stop
```

- Redis 命令
```
启动: /usr/local/bin/redis-server /etc/redis.conf
进入: redis-cli -a admin
```

- MySql 命令
```
启动: mysqld
```