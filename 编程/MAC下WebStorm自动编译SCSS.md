### 前言

SCSS 是 SASS 3 引入新的语法，其语法完全兼容 CSS3，并且继承了 SASS 的强大功能。

### 安装ruby

MAC下安装如下，其他系统请参考[官网](https://www.ruby-lang.org/zh_cn/documentation/installation/)

```
brew install ruby
```

### 安装SASS

```
gem install sass
```

### 安装Compass

Sass本身只是一个编译器，Compass在它的基础上，封装了一系列有用的模块和模板，补充Sass的功能。它们之间的关系，有点像Javascript和jQuery。

```
# 默认
gem install compass
gem install compass --add https://rubygems.org/
# 安装某个源
gem install compass --source https://rubygems.org/
# 添加源
gem sources --add https://rubygems.org/
gem sources --add https://ruby.taobao.org/
# 删除源
gem sources --remove https://rubygems.org/
gem sources --remove https://ruby.taobao.org/
```

### Webstorm Scss 自动编译

1. Preferences -> Tools -> File Watchers
2. 点击 + 号，选择 SCSS
3. 配置如下(我们把 `.css` 文件以及 `.css.map` 跟 `.scss` 文件同一目录下)
```
# 注意:每个配置后面都有插入宏命令选项 insert macros 
# 配置 Program:
C:\Ruby21-x64\bin\scss.bat    # windows系统下scss的安装路径
/usr/local/bin/scss           # mac ox 系统下scss的安装路径
# 配置 Arguments:
--no-cache --update $FileName$:$FileNameWithoutExtension$.css
# 配置 Working directory:
$FileDir$
# 配置 Output paths to refresh:
$FileNameWithoutExtension$.css:$FileNameWithoutExtension$.css.map
# 注意
$FileParentDir$ 表示的是 scss 文件所在的文件夹的父级文件夹
```

![自动编译](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/webstorm-scss-to-css.png)

### 压缩
SCSS自动编译为SCSS后，上线之前我们需要压缩CSS文件以及JS文件，以便于文件更小，加载速度更快。因此我们用到了[yuicompressor](https://github.com/yui/yuicompressor/releases/)来实现WebStorm自动压缩功能。

1. [下载](https://github.com/yui/yuicompressor/releases/)
2. Preferences -> Tools -> File Watchers
3. 点击 + 号，选择 YUI Compressor CSS
4. Program 写入 yuicompressor.jar 的路径即可
5. JS 压缩同上。以后再写SCSS和JS文件，会自动生成`.min.css`和`.min.js`文件

![YUI CSS](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/webstorm-yui-compressor-scss.png)

![YUI JS](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/webstorm-yui-compressor-js.png)

### 参考

- [参考一](http://jinyanhuan.github.io/2015/04/03/sass-in-webstorm/)
- [参考二](http://www.cnblogs.com/wind128/p/4226318.html)
- [Sass](http://www.ruanyifeng.com/blog/2012/06/sass.html)
- [Compass](http://www.ruanyifeng.com/blog/2012/11/compass.html)