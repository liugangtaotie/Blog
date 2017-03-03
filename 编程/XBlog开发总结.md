## 前言
个人博客历史：[博客之旅](https://yuchao.wang/article?id=31)

在用 Hexo 的过程中，发现没有办法更改分类名以及标签名，而且还没有办法把标签分类，因此想开发一个「分类-标签」结构的博客系统，可以参考[segmentfault](https://segmentfault.com/tags)，后来发现 Typecho 的分类选项是可以多选的，因此 Typecho 是间接实现了这个功能。因此在所有的博客系统中，其实 Typecho 是最符合我的个人需求的博客系统。那我为什么又继续开发了 XBlog 呢？

- 学习并整理前端、后端、数据库、客户端、运维等方面的知识
- 开发自己的移动客户端，方便自己浏览以及后台操作。
- 统一优化前后端，提升加载速度。（备注：目前只发现 [Typecho博客系统](http://typecho.org/) 下的 [Linpx](https://www.linpx.com/) 及 [Firekylin博客系统](https://github.com/75team/firekylin) 下的 [QuQu](https://imququ.com/) 可以实现秒开）

## 需求

- **初衷**

 + **功能上**：登录、发表、查看、修改、删除、评论、分类、标签、多用户、搜索、文件上传、Markdown支持、固定连接、移动适配、邮箱验证、全自动安装
 + **内容上**：关于、友链、其他、全部是文章的一种。

- **版本1.0**

 + **注意**：以下功能XBlog数据库在最初设计中全部存在，因此框架中如果后续需要可以扩展。
 + **评论**：为了提高交友质量和评论质量，以及提高加载速度，因此去除评论功能。
 + **用户**：去除了评论，因此也就不存在多用户了，是单用户博客系统。
 + **文件**：为了防止占用服务器的带宽，因此决定外链静态资源文件，去除文件上传功能。

## 设计
- **UI**：字体、颜色、大小、适配
- **分类标签**：分类只有一级，每个标签
- **数据库**：表结构设计、创建数据库脚本

## 开发

- **HTTL**:Java模板引擎，依赖httl-1.0.11.jar、httl-servlet-1.0.11.jar
- **Slf4j**:logback、logback-core、logback-classic(相当于logback与SLF4J的桥接包，这样会造成重复，可以吧simple去掉)
- **HikariCP**:数据库连接池，依赖：slf4j-api-1.7.21.jar、slf4j-simple-1.7.21.jar、mysql-connector-java-5.1.39-bin.jar
- **JedisPool**:Redis池
- **SCSS**
- **Lazysizes.min.js**:延迟加载图片
- **Marked.min.js**:markdown语法解析器
- **Toc.min.js**:目录生成器
- **Session**:IP+浏览器 生成 Session
- **Yui压缩**:压缩CSS及JS文件
- **Properties**:文件读写

## 测试

- 本地使用 JMeter 测试，并发量250左右，同Tomcat并发量、MySql并发量、ThreadPool、JVM、RedisPool相关。
- 未完待续...

## 运维

- 在腾讯云主机上 Centos 6.5 系统下，搭建并配置 MySQL、Nginx、Tomcat、Redis、Https、Java

## 最终

- **域名**：万网
- **主机**：腾讯云
- **代理**：Nginx
- **服务器**：Tomcat
- **数据库**：MySQL
- **缓存**：Redis
- **存储**：阿里云 OSS
- **Https**:Let's Encrypt

## 优化

- **加快访问速度** 
 + 将 Tomcat + Nginx 动静分离
 + 静博客系统中的静态资源放到OSS中。
 
- **Redis数据同步**
 + 在多台Tomcat负载的时候，由于内存中缓存了Redis的数据，因此会出现Redis数据不同步问题，影响分类和标签功能。

## 总结

最初用Hexo不爽的时候，想换成 Typecho 来着，后来由于上半年使用AngularJS和Ionic开发了一段时间的Mobile Web APP，学了一些前端JS+CSS的知识。这样就想着后端、前端、客户端都懂点，不如自己开发一款个人博客系统。除了Android以外，虽然所有的技术自己都懂一点，但是毕竟很多技术自己没真正接触过，所以没想着能完全做出来，从最初博客的需求、功能、样式、颜色、字体、大小等的设计，到后来的程序开发测试，再到云主机上Centos 6.5 下面的环境搭建、应用部署，遇到了许许多多的问题（整理中），竟然被我逐个解决了（这里必须要感谢王忠给了我技术上的支持）。

完成这个博客系统1.0版本后，给了自己很强的自信心，让我觉得只要能持续学习，把难点分解开逐个攻克，就能实现自己的目标。

**心亘所向，无事不成！**