## Android混淆

下面都是根据自己做过的一些项目，优化整理出来的，如果有不对的地方，欢迎指出。

```
-optimizationpasses 5                                                           # 指定代码的压缩级别
-dontusemixedcaseclassnames                                                     # 是否使用大小写混合
-dontskipnonpubliclibraryclasses                                                # 是否混淆第三方jar
-dontpreverify                                                                  # 混淆时是否做预校验
-verbose                                                                        # 混淆时是否记录日志
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*        # 混淆时所采用的算法

-keep public class * extends android.app.Activity                               # 保持哪些类不被混淆
-keep public class * extends android.app.Application                            # 保持哪些类不被混淆
-keep public class * extends android.app.Service                                # 保持哪些类不被混淆
-keep public class * extends android.content.BroadcastReceiver                  # 保持哪些类不被混淆
-keep public class * extends android.content.ContentProvider                    # 保持哪些类不被混淆
-keep public class * extends android.app.backup.BackupAgentHelper               # 保持哪些类不被混淆
-keep public class * extends android.preference.Preference                      # 保持哪些类不被混淆
-keep public class com.android.vending.licensing.ILicensingService              # 保持哪些类不被混淆

-optimizationpasses 5                                                           # 指定代码的压缩级别
-dontusemixedcaseclassnames                                                     # 是否使用大小写混合
-dontskipnonpubliclibraryclasses                                                # 是否混淆第三方jar
-dontpreverify                                                                  # 混淆时是否做预校验
-verbose                                                                        # 混淆时是否记录日志
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/*        # 混淆时所采用的算法

-keep public class * extends android.app.Activity                               # 保持哪些类不被混淆
-keep public class * extends android.app.Application                            # 保持哪些类不被混淆
-keep public class * extends android.app.Service                                # 保持哪些类不被混淆
-keep public class * extends android.content.BroadcastReceiver                  # 保持哪些类不被混淆
-keep public class * extends android.content.ContentProvider                    # 保持哪些类不被混淆
-keep public class * extends android.app.backup.BackupAgentHelper               # 保持哪些类不被混淆
-keep public class * extends android.preference.Preference                      # 保持哪些类不被混淆
-keep public class com.android.vending.licensing.ILicensingService              # 保持哪些类不被混淆

# 保持 native 方法不被混淆
-keepclasseswithmembernames class * {
    native <methods>;
}

# 保持自定义控件类不被混淆
-keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet);
}

# 保持自定义控件类不被混淆
-keepclasseswithmembers class * {
    public <init>(android.content.Context, android.util.AttributeSet, int);
}

# 保持自定义Activity不被混淆
-keepclassmembers class * extends android.app.Activity {
   public void *(android.view.View);
}

# 保持枚举 enum 不被混淆
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

# 保持 Parcelable不被混淆
-keep class * implements android.os.Parcelable {
  public static final android.os.Parcelable$Creator *;
}

# 保持自定义控件类不被混淆
-keep public class * extends android.view.View {
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
    public void set*(...);
}

# 保持 Parcelable 不被混淆
-keep class * implements android.os.Parcelable {
  public static final android.os.Parcelable$Creator *;
}
-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}


# 保持使用 webview 的类
-keepclassmembers class fqcn.of.javascript.interface.for.webview {
   public *;
}
-keepclassmembers class * extends android.webkit.WebViewClient {
    public void *(android.webkit.WebView, java.lang.String, android.graphics.Bitmap);
    public boolean *(android.webkit.WebView, java.lang.String);
}

# 生成的R资源
-keepclassmembers class **.R$* {
    public static <fields>;
}

# 保持Android自带的
-dontwarn android.support.**
 -keep class android.support.**{*;}

# 不优化输入的类文件
-dontoptimize
# 将.class信息中的类名重新定义为"SourceFile"字符串
-renamesourcefileattribute SourceFile

# 并保留源文件名为"Proguard"字符串，而非原始的类名 并保留行号
-keepattributes SourceFile, LineNumberTable

-keepattributes *Annotation*
-keepattributes Signature
-keepattributes InnerClasses

# VIP Add the libraries, but ignore warnings about them
-ignorewarnings 

# 解决这个问题=> Error:warning: Ignoring InnerClasses attribute for an anonymous inner class
-keepattributes EnclosingMethod 

#================================== 下面都是可以自定义的 ==================================# 

# 保持自己定义的类不被混淆
-keep class MyClass;

# keep 其他 第三方 类库
-libraryjars libs
-dontwarn com.jeremyfeinstein.slidingmenu.lib.**
-keep class com.jeremyfeinstein.slidingmenu.lib.**{*;}

# keep 使用 泛型 的类

# keep 使用 反射机制 的类

```