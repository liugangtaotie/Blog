## 环境
- CentOS 6.5 
- MySQL 5.6.35

## 安装
**安装指定版本5.6**

1. 检测是否已安装
```
rpm -qa|grep mysql
find / -name mysql
```

2. 删除已安装的
```
yum remove mysql-*
rm -rf /var/lib/mysql
``` 

3. 安装
```
rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
yum install mysql-community-server
```

## 启动
```
启动 service mysqld start
停止 service mysqld stop
版本 mysql -V
初始化密码 /usr/bin/mysqladmin -u root password
登录 mysql -u root -p
```

## 密码

### 修改密码

```
初始化密码 /usr/bin/mysqladmin -u root password
修改密码   /usr/bin/mysqladmin -u root -p password
```

### 重置密码

前两天在远程调试Web项目时，出现了这个错误，经查找资料发现需要重置密码。

```
Access denied for user 'root'@'localhost' (using password: YES)
```

1. 打开配置文件 `vim /etc/my.cnf`
2. 最后下面添加 `skip-grant-tables` 保存并退出
3. 重启服务 `service mysqld restart`
4. 进入MySQL数据库 `mysql -u root -p` 不输入密码
```
mysql> use mysql;
mysql> update user set password=PASSWORD("this_is_your_new_password") where user='root';
```
5. 删除刚才添加的 `skip-grant-tables`
6. 重启服务 `service mysqld restart`
7. 重新进入 `mysql -u root -p` 输入密码检测是否成功

## 远程连接

> 注意：以下MySQL操作的均为root用户，不建议开启root用户的远程访问权限

### 开启

1. 配置云服务器厂商的端口安全设置，允许3306端口访问：比如腾讯云需要配置安全组
2. 开启3306端口
```
/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
```

3. 重启防火墙，使之生效
```
保存 /etc/rc.d/init.d/iptables save
查看 /etc/init.d/iptables status
重启 /etc/init.d/iptables restart
```

4. 进入MySQL
```
方式一 mysql -u root -p
方式二 mysql -uroot -pYour_Pass_Word
```

5. 设置MySQL允许远程连接
```
mysql> show databases;
mysql> use mysql;
mysql> grant all privileges on *.* to 'root'@'%' identified by 'this_is_my_password' with grant option;
mysql> update user set host='%' where user='root'; //%是允许所有IP，也可以指定具体IP
mysql> flush privileges;
```

6. 远程连接，其中`your_ip_address`替换为你的IP地址即可。
```
mysql -h your_ip_address -P 3306 -u root -p
```

### 关闭
1. 配置云服务器厂商的端口安全设置，禁止3306端口访问：比如腾讯云需要配置安全组
2. 关闭3306端口
```
/sbin/iptables -I INPUT -p tcp --dport 3306 -j DROP
```

3. 重启防火墙，使之生效
```
保存 /etc/rc.d/init.d/iptables save
查看 /etc/init.d/iptables status
重启 /etc/init.d/iptables restart
```

4. 进入MySQL
```
mysql -u root -p
```

5. 设置MySQL禁止远程连接，把`host`的值重置为`localhost`
```
mysql> use mysql;
mysql> update user set host='localhost' where user='root';
mysql> flush privileges;
```

## 内存优化

MySQL启动后，默认占用较大内存（512MB），我们优化一下默认的内存占用

1. 修改配置文件
```
vim /etc/my.cnf
```

2. 添加下面内容在[mysqld]后面
```
port=3306
character-set-server=utf8
performance_schema_max_table_instances=200
table_definition_cache=200
table_open_cache=128
join_buffer_size=64M
```

## 本站

本站也开启了MyQL 3306端口以及远程访问权限，不过，本站不怕破解，原因有以下几点

1. 个人站点较小
2. 腾讯云安全监测
3. 随时可关闭访问权限
4. 站点内容 Github 及 本地都有备份
5. 不定时备份本站数据库