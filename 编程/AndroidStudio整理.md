### 快捷键(MAC版)
- cmd + alt + L 格式化
- cmd + H 类层级
- ctrl + alt + O 清除无效包
- cmd + delete 删除行
- cmd + alt(范围：是否全局) +F/R 查找替换
- ctrl + O 快速复写方法
- cmd + shift + O 大小写
- ctrl + space 智能提示(可能于MAC下冲突，偏好设置-键盘中修改即可)
- alt + enter 强制转换提示
- ctrl + shift(范围：是否全部) + plus/minus 折叠/展开
- alt + shift + up/down 上/下移动
- alt + cmd + T 快速生成一些方法
- cmd + shift(范围：是否全部) + O 查找文件
- shift + shift 快速查找
- fn + cmd + F12 快速显示方法
- ctrl + T 重命名
- cmd + U 查看父类
- cmd + shift + E 查看最近修改
- cmd + ctrl + up 定位文件
- cmd + , Preference
- cmd + shift + J 生成注释（可能跟MAC冲突，需要自定义）
- cmd + N 快速生成方法(set/get/consta/equals/toString)
- cmd + [ 鼠标上一个位置
- cmd + ] 鼠标下一个位置

### 常用插件
- LayoutCreator
- Genymotion
- Android Parcelable code generator
- Bash Support
- GsonFormat
- Markdown
- Idea-markdown
- ButterKnife Zelezny
- Android Studio Prettify
- SelectorChapek
- Android Drawable Importer
- IdeaVim
- Eclipse Code Formatter
- Android WiFi ADB
- GsonFormat
- [Power Mode II](https://github.com/axaluss/power-mode-intellij-plugin)
- [LeakCanary](http://www.liaohuqiu.net/cn/posts/leak-canary-read-me/)
- Android Code Generator
- Android Methods Count
- Lifecycle Sorter
- CodeGlance
- findBugs-IDEA
- AndroidPixelDimenGenerator
- JsonOnlineViewer
- Android Styler
- Android Drawable Importer
- SelectorChapek for Android
- GradleDependenciesHelperPlugin
- AndroidProguardPlugin
- Android-DPI-Calculator

### 小技巧

- 抽取style：`光标位于布局文件目标控件中 -> 右键 -> Refactor -> Extract -> Style`
- 分屏：`window -> Editor Tables -> split`
- 每一行代码版本记录：`行号右边空白处右键 - Annotate`
- 代码分析：`Analysis -> Inspect Code`
- 动态修改方法签名：`Refactor -> Change Signature` 
- 等号对齐：`setting—>code style—>Groovy—>Wrapping and baces—>field groups—>align in columns`

### 错误合集

- xxx.apk does not exist on disk
```
参考: http://stackoverflow.com/questions/34039834/the-apk-file-does-not-exist-on-disk
1.clean project
2.restart android studio 
3.gradle -> refresh all gradle project !
```

- 修改代码之后运行，AndroidStudio没有进行重新编译，而是提示no changes to deploy。
```
1.settings->Build,Execution,Deployment->Instant Run
2.将 Enable Instant Run to hot swap code/resource changes on deploy 选项的勾去掉
```

- Debug时Warning: No executable code found at line
```
1.clean
2.rebuild
3.File->Invalidate Cache/Restart
```

- Plugin is too old, please update to a more recent version, or set ANDROID_DAILY_OVERRIDE environment
 + 解决办法：升级到最新版本的Gradle即可

- Unable to make the module: related gradle configuration was not found. Please, re-import the Gradle project and try again
 + 解决办法：打开gradle视图，刷新一下即可。

- can't rename root module
 1. 关闭Android Studio，然后重命名系统中项目文件夹名称，然后重新导入。
 2. 修改根目录下的.iml文件名为[NewName].iml，及该文件中的`external.linked.project.id=[NewName]`
 3. 修改.idea/modules.xml里面的`<module fileurl="file://$PROJECT_DIR$/[NewName].iml" filepath="$PROJECT_DIR$/[NewName].iml" />`

- SVN文件无法提交（2种解决办法）
 + 配置ignore Files
 + svn add liblocSDK6a.so --force