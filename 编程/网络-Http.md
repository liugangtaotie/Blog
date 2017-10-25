## 概述

HTTP（Hypertext Transfer Protocol）超文本传输协议，它是现代互联网最重要也是最基本的协议。Http协议是无状态的应用层协议。

![HTTP协议](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/internet-http-01.png)

### URI

统一资源标识符（Uniform Resource Identifier)是一个用于标识本地及网络资源名称的字符串。格式如下

```
<协议>://<主机>:<端口>/<路径>/
```

### URL

统一资源定位符（uniform resource locator），URL 是一种特殊的 URI ，对可以从互联网上得到的资源的位置和访问方法的一种简洁的表示，是互联网上标准资源的地址，算是 URI 的一种实现。

一个完成的 URL 如下

```
https://www.ctolib.com/android/topics-112725.html?name=wangyuchao&articleId=3#title1
```

根据 URI 抽象出来即为

```
scheme://host:port/path?param1=value1&param2=value2#anchor
//////////////////////////////////////////////////////////
scheme     协议
host       主机IP地址
port       端口号
path       虚拟目录
url-params 请求参数
anchor     锚
```

- 协议：http,https,ftp 等互联网协议，`://`为分隔符
- 主机：主机IP，如果主机后有端口号，则需要在域名后面加上分隔符`:`
- 端口：主机IP的某个端口
- 域名：为了更容易标识互联网上的服务器，因此把主机和端口映射为域名。
- 虚拟目录：从第一个`/`到最后一个`/`，虚拟目录为非必须。
- 文件名：从域名之后最后一个`/`到`?`为止，如果没有`?`，则到`#`为止，如果也没有`#`，则到`结束`为止，`?`和`#`之间的是非必须的请求参数集合，参数之间用`&`作为分隔符。
- 锚：从`#`开始到最后

## Http 报文结构

Http有两种报文

- 请求报文：请求行、首部行、实体主体
- 响应报文：状态行、首部行、实体主体

![Http报文结构](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/internet-http-02.png)

### 请求报文-请求行
### 请求报文-首部行(报头)
### 请求报文-实体主体
### 响应报文-状态行
### 响应报文-首部行(报头)
### 响应报文-实体主体
### 报文-首部行(报头)


## 番外

简单说明一下

get请求最多1024字节

Header

Session

Cookie

Request

Https 

## 参考

- 《计算机网络》
- [维基百科](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)
- [HTTP协议详解(真的很经典)](https://www.cnblogs.com/li0803/archive/2008/11/03/1324746.html)
- [Http协议详解(超详细)](http://www.cnblogs.com/wangning528/p/6388464.html)
- [文加图,理解Http请求与响应](http://blog.csdn.net/oncealong/article/details/52087284)
- [Http协议详解(阿里云)](https://yq.aliyun.com/articles/5892)
- [关于Http协议，一篇就够了](http://www.cnblogs.com/ranyonsue/p/5984001.html)