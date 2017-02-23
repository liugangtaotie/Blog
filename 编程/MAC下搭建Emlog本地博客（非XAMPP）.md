---
title: MAC下搭建Emlog本地博客（非XAMPP）
date: 2015-07-16 15:50:39
categories: 
- 编程之美
tags: 
- 教程
permalink: build-emlog-blog-at-local-on-mac-not-xampp

---

**由于php和apache是mac自带的，因此我们只需要phpadmin和mysql以及mysqlworkbench**

### 启动Apache
- 方法一：打开网络共享，打开"系统偏好设置"->"共享"，在"互联网共享"那一项前面打√。
- 方法二：打开终端，输入sudo apachectl start回车输入密码
	+ 再输入sudo apachectl －v 可以查看到Apache的版本信息
	+ 在浏览器中输入http://localhost，会出现It works！的页面

### 运行PHP
1. 打开Finder，选择"前往"-"前往文件夹"，输入"/etc/apache2/"，找到"httpd.conf"文件，打开编辑，搜索#LoadModule php5_module libexec/apache2/libphp5.so把#号去掉
2. 重启Apache，在终端输入sudo apachectl restart
3. 把/Library/WebServer/Documents/index.html.en复制一份改为info.php
4. 打开info.php，在It works后面加上<?php phpinfo(); ?>,然后重启Apache，在浏览器中输入http://localhost/info.php，会出现一个显示php信息的页面。

### 操作MySql
1. 安装完MySql以后，先添加命令到bash PATH中：`/usr/local/mysql/bin/`
2. 命令行启动mysql命令（也可在系统中启动）：`sudo /usr/local/mysql/support-files/mysql.server [start,stop,restart]`
3. 初始化密码命令：`mysqladmin -u root password 初始密码`
4. 修改密码命令（这步暂时用不到，初始是没有密码的，所以省略掉了-p ）：`mysqladmin -u root -p 老密码 password 新密码`
5. 登陆： `mysql -u root -p` 回车后输入密码
6. 查看数据库（登陆成功是4个，否则是2个）后退出：`show databases;exit;`

### MySqlAdmin安装
1. 解压文件后把mysqladmin文件夹拷贝到/Library/WebServer/Documents/中，并全部设置权限为读和写，并且应用在子文件，
2. 把文件夹中config.sample.inc.php命名为config.inc.php并且编辑它将`$cfg['Servers'][$i]['host'] = 'localhost';`中的`localhost`改为`127.0.0.1`
3. 浏览器打开localhost/mysqladmin使用账号密码登陆成功即可

### 安装emlog
安装emlog的时候需要先创建一个数据库，然后在安装界面需要把localhost改为127.0.0.1，然后按部就班安装即可。

**注：此贴基本上对Wordpress,Emlog,Ghost,Typecho,等一系列的PHP博客程序都有效**