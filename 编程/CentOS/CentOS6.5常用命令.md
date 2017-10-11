### 系统

下面罗列的都是一些常用的命令

- **登录**
```
登录 > ssh your_name@your_ip your_password
关机 > shutdown -s -t 10
关机 > shutdown now
重启 > shutdown -r now
重置 > passwd
用户 > whoami
```

- **内存**
```
free -m
```

- **缓存**
```
清空缓存 echo 3 > /proc/sys/vm/drop_caches
同步本地 sync
```

- **安装**
```
yum install wget
yum install tree
```

- **yum**
```
备份 mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
更新 wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
更新 yum clean all
更新 yum makecache
升级 yum -y update
```

- **端口占用**
```
查找 netstat -apn|grep 80
查看 ps -aux|grep <pid>
杀死 kill -9 <pid>
```

- **防火墙**
```
# 开启端口
/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 443 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 6379 -j ACCEPT
# 关闭端口
/sbin/iptables -I INPUT -p tcp --dport 3306 -j DROP
# 保存 
/etc/rc.d/init.d/iptables save
# 查看 
/etc/init.d/iptables status
# 重启 
/etc/init.d/iptables restart
```

- **redis**
```
安装 > yum install redis
启动 > service redis start
重启 > service redis restart
进入 > /usr/bin/redis-cli -a -password
# 配置文件
1.打开文件 vim /etc/redis.conf
2.添加密码 将 #requirepass foobared 改为 requirepass your_password
3.开启远程 配置 bind 选项
4.重启服务 service redis restart
```