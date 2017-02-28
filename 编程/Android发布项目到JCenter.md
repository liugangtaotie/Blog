### JCenter是什么

大家应该都用过各种各样的Github上的第三方开源组件。类似这种效果的

```
compile 'wang.yuchao.demoforjcenterlibrary:DemoForJCenterLibraryModel:1.2.5'
```

**我们会比较好奇Android Studio 是从哪里得到这个类库的？**

Android Studio是从build.gradle里面定义的Maven仓库服务器上下载library的。Apache Maven是Apache开发的一个工具，提供了用于贡献library的文件服务器。但是由于maven仓库对开发人员不友好（[原因在此](http://www.devtf.cn/?p=760)），因此Android Studio 团队把默认仓库换成了JCenter。JCenter是一个由bintray.com维护的Maven仓库。我们在项目的build.gradle 文件中如下定义仓库，就能使用jcenter了。

```
allprojects {
    repositories {
        //mavenCentral()
        jcenter()
    }
}
```

为了更好的描述他们之间的关系，以及怎么发布一个项目到JCenter上，我下面用一个例子进行演示一下。

### 演示：发布项目到JCenter

#### 第一步：准备工作

1. **前言：本机环境**
	- MAC OS 10.10.5
	- Android Studio 2.0
2. **新建一个Android Studio Project** 
	- Application Name -> `DemoForJCenter`
	- 根据 Application Name 会自动生成 Package Name 为 `wang.yuchao.demoforjcenter`
3. **添加一个Android Library 类型的 Model**
	- Model Name -> `DemoForJCenterLibraryModel`
	- Application/Library name -> `DemoForJCenterLibrary`
	- 根据 Application/Library name 可以自动生成 Model Package Name 为 `wang.yuchao.demoforjcenterlibrary`
	- 我们在此 Model 下面新建一个类 ToastUtil.java 用来测试

```java
package wang.yuchao.demoforjcenterlibrary;

import android.content.Context;
import android.widget.Toast;

public class ToastUtil {
    public static void show(Context context, String message) {
        Toast.makeText(context, "DemoForJCenterLibrary：" + message, Toast.LENGTH_SHORT).show();
    }
}
```

#### 第二步：网站配置

1. **Push 工程到 Github**
2. **登录 https://bintray.com/ 并 Add New Package**

	+ 方式一：点击 maven -> Import GitHub repositories 即可把Github的项目导入。（导入后Bintray生成的项目名默认是Github的项目名）
	+ 方式二：点击 maven -> Add New Package 后，根据提示写入各种信息后Create Package。（Version control 是必填项）

**注意：为了详细的演示与讲解，我们使用上述方式二进行配置，并且为了区分Bintray上的Name跟Github默认的项目名，Bintray Name我们设置的不是Github默认的DemoForJCenter，而使用的是 DemoForJCenterName 。如下图。(但是建议：设置为Github默认的)**

![截图](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-jcenter.jpg)

#### 第三步：本地配置

**1. 打开本地Project的local.properties，最后添加**

```
bintray.user=YOUR_BINTRAY_USERNAME
bintray.apikey=YOUR_BINTRAY_API_KEY
```

- `YOUR_BINTRAY_USERNAME`是你在 https://bintray.com/ 的用户名
- `YOUR_BINTRAY_API_KEY`是你在 https://bintray.com/ 的 `API_KEY`（点击右上角用户名->your profile->edit ->Api Key）

**2. 打开本地Project的gradel文件，dependencies节点下添加一些插件**

```
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.4'
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.5'
classpath "org.jfrog.buildinfo:build-info-extractor-gradle:3.1.1"
```

**3. 打开本地要上传的Model（即DemoForJCenterLibraryModel）下的gradle文件，添加如下代码到最后**

```
apply plugin: 'com.android.library'

ext {
    bintrayRepo = 'maven'
    bintrayName = 'DemoForJCenterName'

    publishedGroupId = 'wang.yuchao.demoforjcenterlibrary'

    libraryName = 'DemoForJCenterLibrary'
    artifact = 'DemoForJCenterLibraryModel'

    libraryDescription = 'This library is test for DemoForJCenter'

    siteUrl = 'https://github.com/yuchao-wang/DemoForJCenter'
    gitUrl = 'https://github.com/yuchao-wang/DemoForJCenter.git'

    libraryVersion = '1.2.3'

    developerId = 'wangyuchao'
    developerName = 'yuchao-wang'
    developerEmail = '1154786190@qq.com'

    licenseName = 'The Apache Software License, Version 2.0'
    licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
    allLicenses = ["Apache-2.0"]
}

apply from: 'https://raw.githubusercontent.com/nuuneoi/JCenter/master/installv1.gradle'
apply from: 'https://raw.githubusercontent.com/nuuneoi/JCenter/master/bintrayv1.gradle'
```

**3.1 参数说明**

|参数|说明|
|---|---|
|bintrayRepo|`https://bintray.com/` 下的仓库名|
|bintrayName|`https://bintray.com/` 仓库下的项目名|
|publishedGroupId|Model Package Name|
|libraryName|Model Application/Library name（没啥用）|
|artifact|Model 名|
|libraryVersion|Model 版本号|

**3.2 使用说明**

```
compile 'publishedGroupId:artifact:libraryVersion'
```

#### 第四步：发布项目

**1. 执行**

```
./gradlew install
```

**2. 执行**

```
./gradlew bintrayUpload
```

**3. 打开 https://bintray.com/ ，你会发现最新版本已经上传**

到目前为止，你已经成功地把类库文件上传到bintray上，接下来就是同步到jcenter上了。

**4. 点击 Add to JCenter** 等待JCenter审核即可。
### 测试一下

在等待审核的过程中，你可以先修改Project下的gradle文件进行测试。如果审核通过了以后，下面的maven节点也就可以注释掉了。

```
allprojects {
    repositories {
        maven {
            // your bintray maven address . you can find it on 
            url 'https://dl.bintray.com/yuchao-wang/maven'
        }
        jcenter()
    }
}
```

app model 下的gradle文件引入

```
compile 'wang.yuchao.demoforjcenterlibrary:DemoForJCenterLibraryModel:1.2.3'
```

### 番外篇

**1. 踩过的坑**

如果上传的过程中出现

Could not upload to 'https://xxxxxxx.pom': HTTP/1.1 400 Bad Request [message:Unable to upload files: Maven group, artifact or version defined in the pom file do not match the file path 'xxxxxxx.pom']

是因为module的名字和Model下gradle文件配置的artifact不一致导致的

**2. 删除Bintray项目**

Edit Your Profile -> Repositories -> 

### 参考

- http://inthecheesefactory.com/blog/how-to-upload-library-to-jcenter-maven-central-as-dependency/en
- http://blog.csdn.net/wangdong20/article/details/50098535
- http://www.jianshu.com/p/4a693337e3bd
- http://www.jianshu.com/p/0e7b8e14f0cd/comments/1050253

**[项目传送门](https://github.com/yuchao-wang/DemoForJCenter)**