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

### 概览

Http有两种报文

- 请求报文：请求行、首部行、实体主体
- 响应报文：状态行、首部行、实体主体

大致结构如下

```
<start-line>           // 开始行(请求行/响应行)
*(<message-header>)    // 首部行(请求头)
CRLF                   // 回车换行
[<message-body>]       // 实体主体
```

详细结构如下图

![Http报文结构](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/internet-http-02.png)

下面主要讲解一下最常见的开始行(请求行/响应行)以及首部行(Headers)，实体主体略过。

### 请求报文-请求行<Start-Line>

格式如下

```
Method Request-URI HTTP-Version CRLF
```

**Method** 请求方法

- PUT（增）
- DELETE（删）
- POST（改）
- GET（查）
- HEAD（资源元数据/信息）
- TRACE（回显服务器收到的请求，用于测试）
- OPTIONS（查询服务器的性能，或者查询与资源相关的选项和需求）
- CONNECT（预留，HTTP/1.1协议中预留给Http代理服务器，通常用于SSL加密服务器的链接）

用的最多的即 GET 和 POST，HTTP 协议没有对传输数据和 URL 长度进行限制，但特定浏览器和服务器对 URL 长度有限制。因此GET提交时，传输数据有 URL 长度限制；由于 POST 操作不是通过 URL 传值，因此理论上数据长度不受限。

**Request-URI** 请求统一资源标识符
**HTTP-Version** HTTP协议版本
**CRLF** CR 为回车 LF 为换行

### 响应报文-状态行<Start-Line>

格式如下

```
HTTP-Version Status-Code Reason-Phrase CRLF
```

**HTTP-Version** HTTP协议版本


**Status-Code** 服务器响应状态码，具体请看[HTTP状态码](http://tool.oschina.net/commons?type=5)

- 1XX 表示请求已被成功接收，告诉客户端可以继续发送下一个请求了，若如果已发送完毕可以忽略它。
- 2XX 成功；
- 3XX 重定向；
- 4XX 客户端错误；
- 5XX 服务器端错误；

**Reason-Phrase** 状态码的文本描述

**CRLF** CR 为回车 LF 为换行

### 首部行(Headers)

Headers 包括 `General Headers`、`Entity Headers`、`Request/Response Headers` 三种。

每一个报头域都是由【名字 + ":" + 空格 + 值】组成，形式为 `header-name: value`，`header-name`大小写无关

#### General Headers

被 Request/Response 共享的 Headers 成为`General Headers`，具体有以下

```
general-header = Cache-Control           
               | Connection       
               | Date             
               | Pragma           
               | Trailer          
               | Transfer-Encoding
               | Upgrade          
               | Via              
               | Warning
```

- Cache-Control 指定请求和响应遵循的缓存机制。
- Connection 允许客户端和服务器指定与请求/响应连接有关的选项
- Date 提供日期和时间标志,说明报文是什么时间创建的
- Pragma 头域用来包含实现特定的指令，最常用的是Pragma:no-cache
- Trailer 如果报文采用了分块传输编码(chunked transfer encoding) 方式,就可以用这个首部列出位于报文拖挂(trailer)部分的首部集合
- Transfer-Encoding 告知接收端为了保证报文的可靠传输,对报文采用了什么编码方式
- Upgrade 给出了发送端可能想要”升级”使用的新版本和协议
- Via 显示了报文经过的中间节点(代理,网嘎un)

#### Entity Headers

EntityHeaders 主要用来描述消息体（message body）的一些元信息，具体有

```
entity-header  = Allow                   
               | Content-Encoding 
               | Content-Language 
               | Content-Length   
               | Content-Location 
               | Content-MD5      
               | Content-Range    
               | Content-Type     
               | Expires          
               | Last-Modified
```

- 以 Content 为前缀的 Headers 主要描述了消息体的结构、大小、编码等信息
- Expires 描述了 Entity 的过期时间
- Last-Modified 描述了消息的最后修改时间

#### Request Headers

```
request-header = Accept                   
               | Accept-Charset    
               | Accept-Encoding   
               | Accept-Language   
               | Authorization     
               | Expect            
               | From              
               | Host              
               | If-Match          
               | If-Modified-Since 
               | If-None-Match     
               | If-Range          
               | If-Unmodified-Since
               | Max-Forwards       
               | Proxy-Authorization
               | Range              
               | Referer            
               | TE                 
               | User-Agent
```

- 前缀为Accept 的headers定义了客户端可以接受的媒介类型、语言和字符集等
- From, Host, Referer 和 User-Agent 详细定义了客户端如何初始化 Request
- 前缀为 If 的 headers规定了服务器只能返回符合这些描述的资源，否则返回 `304 Not Modified`

#### Response Headers

```
response-header = Accept-Ranges
                | Age
                | ETag              
                | Location          
                | Proxy-Authenticate
                | Retry-After       
                | Server            
                | Vary              
                | WWW-Authenticate
```
- Age 表示消息自server生成到现在的时长，单位是秒
- ETag 是对Entity进行MD5 hash运算的值，用来检测更改
- Location 被重定向的URL
- Server 服务器标识

## 番外1 - Session Cookie Token

### Session

会话，是一种抽象概念。

- 不要混淆 Session 和 Session 实现
- Session 是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中

### Cookie

Cookie 是客户端保存用户信息的一种机制，用来记录用户的一些信息，也是实现 Session 的一种方式。

### Token

令牌，是用户身份的验证方式，最简单的token组成:uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign(签名)

### 区别和联系

由于Http 协议中是无状态的，因此无论是上述那种，都是为了标识特定的客户。

## 番外2 - Https

HTTPS和HTTP的区别主要为以下四点：
- https 协议需要CA证书
- http 是超文本传输协议，是明文传输，https 则是具有安全性的 ssl 加密传输协议
- http 和 https 使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443
- http 的连接简单、无状态；https 协议是由 http + ssl 协议构建的可进行加密传输、身份认证的网络协议，更安全

## 参考

- 《计算机网络》
- [维基百科](https://zh.wikipedia.org/wiki/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E8%BE%93%E5%8D%8F%E8%AE%AE)
- [HTTP协议详解(真的很经典)](https://www.cnblogs.com/li0803/archive/2008/11/03/1324746.html)
- [Http协议详解(超详细)](http://www.cnblogs.com/wangning528/p/6388464.html)
- [文加图,理解Http请求与响应](http://blog.csdn.net/oncealong/article/details/52087284)
- [Http协议详解(阿里云)](https://yq.aliyun.com/articles/5892)
- [关于Http协议，一篇就够了](http://www.cnblogs.com/ranyonsue/p/5984001.html)
- [Session和Cookie区别（知乎）](https://www.zhihu.com/question/19786827)