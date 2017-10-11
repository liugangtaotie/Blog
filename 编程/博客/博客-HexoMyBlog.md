---
title: Hexo我的博客
date: 2016-02-17 09:25:50
categories: 
- 编程之美
tags: 
- 教程
permalink: hexo-my-blog

---

## 2016/07/20
PS:现在已经不上传git了，本地生成以后，直接拷贝到虚机上。

## 为什么选择Hexo
### 为什么要写博客
我以前都是写在本子上的，后来发现随着成长，本子上是远远不够的，为了记录技术成长的点滴，才开始写博客，后来发现还可以锻炼口才，梳理思路，让脑子变得更清晰，还有备忘和交朋友的大用。何乐而不为呢？

### CSDN
说起我的博客选择之路，可能要追溯到2011-2012年了，那时还是在CSDN上写博客一年多，每篇文章几百到几千甚至上万的访问量，对当时的我很有满足感，可是后来为什么不在上面写了呢？我记得当时博客访问量增加到30w以后，CSDN在没有告知我的情况下在博文侧边栏挂载了广告，我跟CSDN的管理人员联系过，我说你们可以把广告挂载在博文最底下，不要影响了用户的阅读心情，他们说是在进行测试，后来过了很久发现都没有撤销的迹象，于是我一气之下把博文删了个精光，全部放到草稿箱了，准备迁移到别的博客。

### 探索第一阶段
然后我试用了新浪博客，网易博客，点点网，准备转移到博客园，但是觉得博客园有点太啰嗦了，什么随笔、文章、日记，把我搞晕了，直接抛弃了。

### 空窗期
没想到这就成了我将近一年没有写博客的开始，一直到14年7月份，基本没有再写过博客，这段时间也比较忙，复习、校招、工作、毕设、毕业。基本上没有闲下来的时候（其实就是懒，也有一部分没有合适的博客平台原因），这段时间，我也研究过wordpress和静态博客Octopress。但是觉得wordpress实在是速度太慢了，Octopress基本完全就是自己折腾了，搞得我心力憔悴，有段时间就写在本地的word文档上。

### 探索第二阶段
又重新开始工作以后，14念10月份吧，觉得这样下去不行，必须写博客记录一下，不然知识学了忘，忘了学，太浪费时间，有博客就可以有备忘的功能。我又准备开始折腾了，买了现在的域名，十月一那段时间就试用了各种博客系统discuz x，wordpress，z-blogphp，boblog，typecho，反复体验，测试，后来不知道在哪个人的友链的文章里面看到Emlog和 http://bbs.emlog.net/thread-34419-1-1.html 我自己体验了以后，就下定决定用Emlog博客。最初买的是Emlog官方的美国空间（为了不备案），速度一点都不快，后来，Emlog官方出了一次事故（被黑了一次），美国空间全部被删光了，而且他们没有在额外的机器备份，也就是说：**我的博客的内容全部被删光了**。Fuck!还好当时我本地有Emlog博客，而且基本是一天一备份。才将我的损失降低到了最低。Emlog官方马上联系我说赠送我额外两年使用期，已经给我开通了。那感觉就像是，如果我银行卡里面的钱全部被盗了，然后银行告诉我给我升级为VIP会员来弥补损失。我说：我以后再也不会用你们的空间了。后来就把域名和空间全部搬家到阿里云万网，还备案了，不得不说阿里云确实稳定且NB，我试用过他们的很多产品，都很不错，没遇到过一次宕机的时候。不过，觉得对于个人博客来说，没有必要买云主机，虚拟主机或者ACE足够。

**PS：个人推荐使用阿里云ACE，相当于SVN，等这个到期了，我就换成ACE**

### Emlog
对它只有一个字「爱」，在我需要记录的时候，它出现了。它优点太多了，直到现在我都深深的爱着它，为了它我还自己编写了一个精简主题。这段时间也是我慢慢开始沉淀的一个过程，平常遇到的一些算法题目，听过的好听的歌曲，优美又有意义的句子，遇到的技术问题，基本都放在上面了，15年一年大概写了60篇博客。我觉得一星期整理出一篇博客就足够了。

### 探索第三阶段
诚然Emlog有很多好处，快且小，但是写作的过程中，还得排版之类的，实在是挺累的。不支持Markdown可愁坏了我，我还想过要不自己从头到尾写个支持Markdown的博客系统或者写个插件，后来发现[知乎上的提问1](https://www.zhihu.com/question/21172379)、[知乎上的提问2](https://www.zhihu.com/question/21981094)。后来试用Ghost,Leanote,farbox,Jekyll,gitblog,MWeb,简书。我觉得不错的有：Leanote、MWeb和简书。再后来，不知道怎么就碰到了Hexo，静态博客，还能永久托管在github，优秀主题也不少，让我一下子就动心了。

### 想明白
在博客整体迁移到Hexo之前，我也想了很多，自己到底想要一个什么样的博客呢？遇到Hexo之前，我没有想明白，遇到Hexo后，我终于想明白了！

#### 必备项

- 速度快
- 支持markdown
- 支持归档、分类、标签、友链、关于、目录
- 支持自定义域名
- 备份（注意站点配置文件不要泄露）
	+ Hexo默认是push `/public` 到Github，生成静态博客。
	+ 自定义push `/sources` 到 Github，文章源文件托管。
	+ 本地备份到云端

#### 非必备项
- 站内搜索？整成固定连接，交给搜索引擎吧，或者用Swiftype，我只想安静的写博客
- 最热文章？评论最多文章？你搜到的就是你所需要的，其他都是浮云
- 好看？要那么多图片干什么，白白耗费流量！简单的就是最好看的，比如水、白纸和空气。
- 代码高亮？无所谓，能看到是代码块就行。
- 支持评论？无所谓，我的博客我做主，有人建议很好，没人建议也得走路。
- 支持访问量统计？无所谓，固然是一种激励，但也不能为了写博客而写博客。

### Hexo
> A fast, simple & powerful blog framework

**以后准备定居Hexo了，总的来说就是：生命在于折腾**

## 博客搭建
### 安装Homebrew
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
### 安装Git
- 方法一：使用Homebrew安装`brew install git`
- 方法二：安装器安装：http://git-scm.com/download/mac
- 方法三：如果已经安装了Xcode则相当于已经安装了git

### 安装Node.js
- 方法一：执行`brew install node`
- 方法二：

```
1. cURL：curl https://raw.github.com/creationix/nvm/master/install.sh | sh
2. Wget：wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
```

安装完成后，重启终端并执行`nvm install 4`即可安装Node.js。

- 方法三：安装程序。https://nodejs.org/en/

### 安装Hexo 3.0
```
npm install -g hexo-cli
```

### 本地建站
```
hexo init <folder>
cd <folder>
npm install
hexo g
hexo server -p 3333
```

浏览器中打开localhost:3333查看是否成功

### Push到Github
#### 创建Repository
必须跟用户名一致：yuchao-wang.github.io

#### 设置SSH Key

**检验是否已存在key**

```
执行如下命令
cd ~
cd .ssh
ls
查看是否已有key文件,
存在key的话都会显示id_rsa.pub和id_dsa.pub这两个文件
没有key什么都不会显示
```

**添加一个SSH Key**

```
执行命令
ssh-keygen -t rsa -C "your_email@mail.com"
这里会提示输入一个文件名来保存ssh key
也可以什么都不输入，使用默认的id_rsa.pub和id_dsa.pub
回车之后需要输入两次密码：你自己平常Push到Github的密码
```

**Github 设置 SSH key**

```
1.登录Github->Settings->SSH keys->Add SSH key
2.打开本地id_rsa.pub文件（注意：不是id_rsa文件），复制所有内容
3.将复制的内容粘贴到刚刚那个页面的key对应的文本框里面,title随便
4.最后输入：ssh -T git@github.com 回车输入密码，会提示是否设置成功！
```

**站点配置Github**

```
1. 执行 npm install hexo-deployer-git --save
2. 修改站点_config.xml文件，如下：
deploy: 
  type: git
  repository: git@github.com:yuchao-wang/yuchao-wang.github.io.git
  branch: master
```

执行 `hexo generate -d` 直接生成并部署到Github上，浏览器中输入yuchao-wang.github.io就能看到效果啦！

## 博客配置
### Hexo常用命令
```
hexo clan （清理）
hexo g （生成）
hexo d （Push到github）
hexo server -p 3333（运行服务）
hexo new post 新文章（新建一个文章）
```

### 一声不「坑」
- yaml语法严格，冒号后面一定要加空格
- hexo:ERROR Deployer not found: github
- 执行`hexo d`没反应

```
1. 执行 npm install hexo-deployer-git --save
2. 修改站点_config.xml文件，如下：
deploy: 
  type: git
  repository: git@github.com:yuchao-wang/yuchao-wang.github.io.git
  branch: master
```

### 图床
**技术类图片压缩后，保存在本地uploads文件夹中；生活类图片，保存在微博**

- 七牛云存储
- 微博

### 推荐主题
yilia，next，Maupassant

**安装主题**

```
git clone https://github.com/heroicyang/hexo-theme-modernist.git themes/modernist
```

### Hexo admin插件

```
npm install --save hexo-admin hexo server -d
```

完成以后输入：`http://localhost:4000/admin`

### Next主题中添加文章统计

**申请Leancloud**

控制台->创建应用->应用数据配置界面->点击左侧的齿轮图标->新建Class->名字为Counter->点击设置->点击应用key->得到app_id和app_key

**打开主题配置文件_config.xml配置**

```
leancloud_visitors:
  enable: true
  app_id: your_app_id
  app_key: your_app_key
```

**应用安全**

点击设置->在`Web安全域名`中输入我们的博客域名，用来保证数据调用的安全

### 其他参考

- 官方文档：https://hexo.io/zh-cn/docs/index.html
- 主题Next文档：http://theme-next.iissnan.com/
- Homebrew：http://brew.sh/index_zh-cn.html
- Node.js：https://nodejs.org
- 夏末：[添加文章统计](http://notes.xiamo.tk/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html)
- 简书：http://www.jianshu.com/p/f66103553c45
- 统计：https://leancloud.cn/
- 极客学院：http://wiki.jikexueyuan.com/project/hexo-document/