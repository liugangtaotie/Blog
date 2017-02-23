### 单元测试
AndroidStuio创建工程完成以后，src目录下会有三个文件夹，androidTest是依赖设备运行的测试代码，main是源文件，test是不依赖设备运行的测试代码。

### 本机环境
- Mac OS 10.10.5 
- AndroidStudio 2.0.0

### 依赖
```
apply plugin: 'com.android.application'

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.2"

    defaultConfig {
        applicationId "com.example.wangyuchao.myapplication"
        minSdkVersion 19
        targetSdkVersion 23
        versionCode 1
        versionName "1.0"

        //test
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    //test
    packagingOptions {
        exclude 'LICENSE.txt'
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.3.0'

    //test
    androidTestCompile 'com.android.support:support-annotations:23.3.0'
    androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.1'
}
```


### test文件夹使用

- 打开src下要测试的java文件
- 鼠标放在类名上，右键->Go To -> Test -> Create New Test
- TestLibrary选Junit4，根据需要选择下面的
	+ setUp:测试前必要信息的初始化
	+ tearDown:测试完成后的回收
	+ show inherited method:显示继承方法
	+ Member下的成员方法
- 点击OK选择test目录
- test文件夹下对应的包名下就生成了可以写单元测试逻辑的测试类
- 运行测试类或者测试方法

### 一些方法的说明和使用

- assertEquals()：结果值与期待值的比较。基本上大部分测试用例都可以通过灵活使用这个方法完成。（注意:期待值可以是某个范围或者某些值）
- assertFalse()和assertTrue()
- assertSame()和assertNotSame()：类似“==”
- assertNull()和assertNotNull()

### androidTest文件夹
同test文件夹使用，不同的是根据需要写一下父类。

### UI测试(欢迎各位补充，未完待续)

http://developer.android.com/training/testing/ui-testing/espresso-testing.html

```java
Espresso.onView(ViewMatchers.withText("Hello,World!")).check(ViewAssertions.matches(ViewMatchers.isDisplayed()));
```
