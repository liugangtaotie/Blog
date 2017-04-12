### 环境

**注意：**：可提前把各个服务对应的端口打开。

- CentOS 6.5
- Nginx 1.10.2
- Apache Tomcat 8.5.5
- Java 8

### Nginx

配置文件路径 `/etc/nginx/nginx.conf` 或 `/etc/nginx/conf.d/default.conf`

- **安装**
```
yum install nginx    # 安装
yum remove nginx     # 卸载
yum info nginx       # 查看
```

- **配置**
```
# Nginx配置选项很复杂，可以在一个文件中配置，也可以根据需要在多个文件分开配置。我们这里暂时按照默认分开配置
1.打开 vim /etc/nginx/conf.d/default.conf
2.注释 listen [::]:80 default_server;
3.重启 service nginx restart
```

- **启动**
```
service nginx start   # 启动
service nginx stop    # 关闭
service nginx restart # 重启
```

此时访问 `http://xxx.com` 会出现nginx的欢迎页面。

### JDK

安装Tomcat前需要安装JDK

- **卸载**
```
rpm -qa | grep -E '^open[jre|jdk]|j[re|dk]' # 查找jdk
yum -y xxx                                  # 删除jdk(xxx代表查找出的名字)
rpm -qa|grep java                           # 查找java
yum -y xxx                                  # 删除java(xxx代表查找出的名字)
```

- **安装**
```
下载 wget this_is_jdk_download_url
安装 rpm -ivh this_is_your_downloaded_jdk_rpm
```

- **验证**
```
java -version
javac
```

### Tomcat

配置文件路径是 `tomcat/conf/server.xml`

- **安装**
```
下载 wget http://mirrors.cnnic.cn/apache/tomcat/tomcat-8/v8.5.11/bin/apache-tomcat-8.5.11.zip
解压 unzip apache-tomcat-8.5.11.zip tomcat
启动 sh program/tomcat8080/bin/startup.sh
验证 此时我们访问 http://xxx.com:8080 就会出现tomcat的欢迎界面
```

- **如果启动多个 Tomcat 服务，只需要拷贝一份，然后更改配置文件中端口号8080即可**
```
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```

- **Nginx 代理 Tomcat : 我们需要使用 Nginx 服务代理转发给不同的 Tomcat 服务。修改配置文件如下（其他的省略）**
```
http {
    upstream tomcats {
        # 注意：可代理多个Tomcat服务 。SESSION 同步问题可以用 ip_hash 解决。 
        # 分配权值
        server 127.0.0.1:8080 weight=1;
        # server 127.0.0.1:8081 weight=1;
        # server 127.0.0.1:8082 weight=1;
    }
    server {
        listen       80;
        server_name  tomcats;
        charset utf-8;
        location / {
            proxy_pass http://tomcats/;
            proxy_set_header   REMOTE-HOST $remote_addr;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_connect_timeout   3;
            proxy_send_timeout      30;
            proxy_read_timeout      30;
        }
    }
    # 注释掉下面这句话
    # include /etc/nginx/conf.d/*.conf;
}
```

重启 Nginx 此时访问 `http://xxx.com` 时也出现 tomcat 的欢迎界面

### Https

#### 申请证书

我们使用[Let's Encrypt](https://letsencrypt.org/)来申请三个月的Https的证书。

- **停止服务**
```
service nginx stop
```

- **生成证书**
```
下载 wget https://dl.eff.org/certbot-auto
权限 chmod a+x certbot-auto
执行 ./certbot-auto
由于 CentOS6.5 Python 版本过低，造成自动生成证书失败，因此选择本地生成证书
执行 ./certbot-auto certonly
选择 standalone
输入 邮箱和域名 直接按照提示 下一步 即可
```

- **说明**

1. 生成的文件全部在 `/etc/letsencrypt/`
2. 生成的证书全部在 `/etc/letsencrypt/live/{domain}/`

- **密钥:可省略**
```
# Nginx 默认使用 Diffiel-Hellman 1024 位交换密钥，为了更加安全我们换成2048（不推荐 4096 耗时耗资源）
cd etc/letsencrypt/live/{domain}/        # 进入证书目录
openssl dhparam -out dh2048.pem 2048     # 生成dh2048.pem
```

- **查看:** `tree /etc/letsencrypt/`
```
/etc/letsencrypt/
├── accounts
│   └── acme-v01.api.letsencrypt.org
│       └── directory
│           └── 71c7f8a214c75a8f9b003f3879809be9
│               ├── meta.json
│               ├── private_key.json
│               └── regr.json
├── archive
│   └── yuchao.wang
│       ├── cert1.pem
│       ├── chain1.pem
│       ├── fullchain1.pem
│       └── privkey1.pem
├── csr
│   └── 0000_csr-certbot.pem
├── keys
│   └── 0000_key-certbot.pem
├── live
│   └── yuchao.wang
│       ├── cert.pem -> ../../archive/yuchao.wang/cert1.pem
│       ├── chain.pem -> ../../archive/yuchao.wang/chain1.pem
│       ├── dh2048.pem
│       ├── fullchain.pem -> ../../archive/yuchao.wang/fullchain1.pem
│       ├── privkey.pem -> ../../archive/yuchao.wang/privkey1.pem
│       └── README
└── renewal
    └── yuchao.wang.conf
11 directories, 16 files
```

#### 配置

- **修改Nginx配置文件**:改动较大，为了省事，我把文件内容全部粘贴过来了。
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /var/run/nginx.pid;
# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;
events {
    worker_connections  1024;
}
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;
    ##############################################
    upstream tomcats {
        # 分配权值如果为了session 可以用ip_hash
        server 127.0.0.1:8080 weight=1;
        # server 127.0.0.1:8081 weight=1;
        # server 127.0.0.1:8082 weight=1;
    }
    server  {
        listen 80;
        server_name yuchao.wang;
        return 301 https://$host$request_uri;
        # return 301 https://$server_name$request_uri;
    }
    server {
        listen 443 ssl http2;
        server_name yuchao.wang;
        charset utf-8;
        keepalive_timeout 70;
        ssl on;
        ssl_certificate        /etc/letsencrypt/live/yuchao.wang/fullchain.pem;
        ssl_certificate_key    /etc/letsencrypt/live/yuchao.wang/privkey.pem;
        ssl_dhparam            /etc/letsencrypt/live/yuchao.wang/dh2048.pem;
        ssl_session_cache    shared:SSL:16m;
        ssl_session_timeout  16m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
        ssl_stapling on;
        ssl_stapling_verify on;
        add_header X-Frame-Options DENY;
        add_header X-Content-Type-Options nosniff;
        add_header Strict-Transport-Security "max-age=63072000; includeSubdomains";
        add_header X-Xss-Protection 1;
        # root         /usr/share/nginx/html;
        location / {
          	###### tomcat config start ######
          	proxy_pass http://tomcats/;
          	proxy_set_header   X-Forwarded-Proto https;
          	port_in_redirect on;
          	proxy_redirect off;
          	proxy_set_header   REMOTE-HOST $remote_addr;
          	proxy_set_header   Host $host;
          	proxy_set_header   X-Real-IP $remote_addr;
          	proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
          	proxy_connect_timeout   3;
          	proxy_send_timeout      30;
          	proxy_read_timeout      30;
          	###### tomcat config end ######
        }
        error_page 404 /404.html;
        location = /40x.html {
        }
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
        }
    }
    ##############################################
    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    # include /etc/nginx/conf.d/*.conf;
}
```

- **修改Tomcat配置文件**
```
# 修改
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" proxyPort="443"/>
# 添加
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11AprProtocol"
               maxThreads="128" scheme="https" secure="true" SSLEnabled="true"/>
# Host 节点下添加
  	<Valve className="org.apache.catalina.valves.RemoteIpValve"
    		portHeader="x-forwarded-port"
    		remoteIpHeader="x-forwarded-for"
    		remoteIpProxiesHeader="x-forwarded-by"
    		protocolHeader="x-forwarded-proto"/>
```

#### 评测

可以去这个[网站](https://www.ssllabs.com/ssltest/analyze.html)评测一下安全指数