## 生命周期

![生命周期图](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-activity-01.jpg)

**由Activity1启动到Activity2**
```
activity1.onPause()
activity2.onCreate()
activity2.onStart()
activity2.onResume()
activity1.onStop()
```

**由Activity2返回到Activity1**
```
activity2.onPause()
activity1.onRestart()
activity1.onStart()
activity1.onResume()
activity2.onStop()
activity2.onDestory()
```

**横竖屏切换：重建生命周期**
```
注：4.0 以下是先 onSaveInstanceState() 再 onPause()
onPause()
onSaveInstanceState()
onStop()
onCreate()
onStart()
onRestoreInstanceState()
onResume()
```

**横竖屏切换：不重建生命周期**
```
android:configChanges="orientation|keyboardHidden|screenSize"
```

## Manifest.xml 中 Activity 节点

```
<activity android:allowEmbedded=["true" | "false"]
          android:allowTaskReparenting=["true" | "false"]
          android:alwaysRetainTaskState=["true" | "false"]
          android:autoRemoveFromRecents=["true" | "false"]
          android:banner="drawable resource"
          android:clearTaskOnLaunch=["true" | "false"]
          android:configChanges=["mcc", "mnc", "locale",
                                 "touchscreen", "keyboard", "keyboardHidden",
                                 "navigation", "screenLayout", "fontScale",
                                 "uiMode", "orientation", "screenSize",
                                 "smallestScreenSize"]
          android:documentLaunchMode=["intoExisting" | "always" |
                                  "none" | "never"]
          android:enabled=["true" | "false"]
          android:excludeFromRecents=["true" | "false"]
          android:exported=["true" | "false"]
          android:finishOnTaskLaunch=["true" | "false"]
          android:hardwareAccelerated=["true" | "false"]
          android:icon="drawable resource"
          android:label="string resource"
          android:launchMode=["standard" | "singleTop" |
                              "singleTask" | "singleInstance"]
          android:maxRecents="integer"
          android:multiprocess=["true" | "false"]
          android:name="string"
          android:noHistory=["true" | "false"]  
          android:parentActivityName="string" 
          android:permission="string"
          android:process="string"
          android:relinquishTaskIdentity=["true" | "false"]
          android:resizeableActivity=["true" | "false"]
          android:screenOrientation=["unspecified" | "behind" |
                                     "landscape" | "portrait" |
                                     "reverseLandscape" | "reversePortrait" |
                                     "sensorLandscape" | "sensorPortrait" |
                                     "userLandscape" | "userPortrait" |
                                     "sensor" | "fullSensor" | "nosensor" |
                                     "user" | "fullUser" | "locked"]
          android:stateNotNeeded=["true" | "false"]
          android:supportsPictureInPicture=["true" | "false"]
          android:taskAffinity="string"
          android:theme="resource or theme"
          android:uiOptions=["none" | "splitActionBarWhenNarrow"]
          android:windowSoftInputMode=["stateUnspecified",
                                       "stateUnchanged", "stateHidden",
                                       "stateAlwaysHidden", "stateVisible",
                                       "stateAlwaysVisible", "adjustUnspecified",
                                       "adjustResize", "adjustPan"] >   
    . . .
</activity>
```

下面我们只讲解几个重点且常见的节点，其他的详见[Android Manifest Activity Element](https://developer.android.com/guide/topics/manifest/activity-element.html)

**android:launchMode**

`standard`：默认，可多次实例化
`singleTop`：同standard，可多次实例化，当配置成singleTop的Activity正好位于栈顶时，不会构造新的实例，即A->A不会构造新的，调用onNewIntent()->onStart()进行复用。
`singleTask`：创建一个实例，检查任务栈是否存在，如果不存在，则创建新的任务栈。如果存在的话，会把该实例任务栈上的Activity全部finish掉，然后调用 onNewIntent()->onStart() 复用。
`singleInstance`：创建一个实例，总是开启新任务栈(不是进程)，它是任务栈中唯一的Activity，任务栈不仅可以跨应用，还可以跨进程，可以理解为整个应用的单例Activity。

**android:alwaysRetainTaskState**

系统是否始终保持 Activity 所在任务栈的状态 —“true”表示保持，“false”表示允许系统在特定情况下将任务重置到其初始状态。 默认值为“false”。该属性只对任务的根 Activity 有意义；对于所有其他 Activity，均忽略该属性。

正常情况下，当用户从主屏幕重新选择某个任务时，系统会在特定情况下清除该任务（从根 Activity 之上的堆栈中移除所有 Activity）。 系统通常会在用户一段时间（如 30 分钟）内未访问任务时执行此操作。

不过，如果该属性的值是“true”，则无论用户如何到达任务，将始终返回到最后状态的任务。 例如，在网络浏览器这类存在大量用户不愿失去的状态（如多个打开的标签）的应用中，该属性会很有用。

**android:clearTaskOnLaunch**

是否每当从主屏幕重新启动任务时都从中移除根 Activity 之外的所有 Activity —“true”表示始终将任务清除到只剩其根 Activity；“false”表示不做清除。 默认值为“false”。该属性只对启动新任务的 Activity（根 Activity）有意义；对于任务中的所有其他 Activity，均忽略该属性。

当值为“true”时，每次用户再次启动任务时，无论用户最后在任务中正在执行哪个 Activity，也无论用户是使用返回还是主屏幕按钮离开，都会将用户转至任务的根 Activity。 当值为“false”时，可在某些情况下清除任务中的 Activity（请参阅 alwaysRetainTaskState 属性），但并非一律可以。

例如，假定有人从主屏幕启动了 Activity P，然后从那里转到 Activity Q。该用户接着按了主屏幕按钮，然后返回到 Activity P。正常情况下，用户将看到 Activity Q，因为那是其最后在 P 的任务中执行的 Activity。 不过，如果 P 将此标志设置为“true”，则当用户按下主屏幕将任务转入后台时，其上的所有 Activity（在本例中为 Q）都会被移除。 因此用户返回任务时只会看到 P。

如果该属性和 allowTaskReparenting 的值均为“true”，则如上所述，任何可以更改父项的 Activity 都将转移到与其有亲和关系的任务；其余 Activity 随即被移除。

**android:finishOnTaskLaunch**

每当用户再次启动其任务（在主屏幕上选择任务）时，是否应关闭（完成）现有 Activity 实例 —“true”表示应关闭，“false”表示不应关闭。 默认值为“false”。

如果该属性和 allowTaskReparenting 均为“true”，则优先使用该属性。Activity 的亲和关系会被忽略。系统不是更改 Activity 的父项，而是将其销毁。

**android:allowTaskReparenting**

当启动 Activity 的任务接下来转至前台时，Activity 是否能从该任务转移至与其有亲和关系的任务 —“true”表示它可以转移，“false”表示它仍须留在启动它的任务处。

如果未设置该属性，则对 Activity 应用由 <application> 元素的相应 allowTaskReparenting 属性设置的值。 默认值为“false”。

正常情况下，当 Activity 启动时，会与启动它的任务关联，并在其整个生命周期中一直留在该任务处。您可以利用该属性强制 Activity 在其当前任务不再显示时将其父项更改为与其有亲和关系的任务。该属性通常用于使应用的 Activity 转移至与该应用关联的主任务。

例如，如果电子邮件包含网页链接，则点击链接会调出可显示网页的 Activity。 该 Activity 由浏览器应用定义，但作为电子邮件任务的一部分启动。 如果将其父项更改为浏览器任务，它会在浏览器下一次转至前台时显示，当电子邮件任务再次转至前台时则会消失。

Activity 的亲和关系由 taskAffinity 属性定义。 任务的亲和关系通过读取其根 Activity 的亲和关系来确定。因此，按照定义，根 Activity 始终位于具有相同亲和关系的任务之中。 由于具有“singleTask”或“singleInstance”启动模式的 Activity 只能位于任务的根，因此更改父项仅限于“standard”和“singleTop”模式。 （另请参阅 launchMode 属性。）

**android:taskAffinity**

与 Activity 有着亲和关系的任务。从概念上讲，具有相同亲和关系的 Activity 归属同一任务（从用户的角度来看，则是归属同一“应用”）。 任务的亲和关系由其根 Activity 的亲和关系确定。

亲和关系确定两件事 - Activity 更改到的父项任务（请参阅 allowTaskReparenting 属性）和通过 FLAG_ACTIVITY_NEW_TASK 标志启动 Activity 时将用来容纳它的任务。

默认情况下，应用中的所有 Activity 都具有相同的亲和关系。您可以设置该属性来以不同方式组合它们，甚至可以将在不同应用中定义的 Activity 置于同一任务内。 要指定 Activity 与任何任务均无亲和关系，请将其设置为空字符串。

如果未设置该属性，则 Activity 继承为应用设置的亲和关系（请参阅 <application> 元素的 taskAffinity 属性）。 应用默认亲和关系的名称是 <manifest> 元素设置的软件包名称。

**android:finishOnTaskLaunch**

每当用户再次启动其任务（在主屏幕上选择任务）时，是否应关闭（完成）现有 Activity 实例 —“true”表示应关闭，“false”表示不应关闭。 默认值为“false”。

如果该属性和 allowTaskReparenting 均为“true”，则优先使用该属性。 Activity 的亲和关系会被忽略。 系统不是更改 Activity 的父项，而是将其销毁。

**android:exported**

Activity 是否可由其他应用的组件启动 —“true”表示可以，“false”表示不可以。若为“false”，则 Activity 只能由同一应用的组件或使用同一用户 ID 的不同应用启动。

默认值取决于 Activity 是否包含 Intent 过滤器。没有任何过滤器意味着 Activity 只能通过指定其确切的类名称进行调用。 这意味着 Activity 专供应用内部使用（因为其他应用不知晓其类名称）。 因此，在这种情况下，默认值为“false”。另一方面，至少存在一个过滤器意味着 Activity 专供外部使用，因此默认值为“true”。

该属性并非限制 Activity 对其他应用开放度的唯一手段。 您还可以利用权限来限制哪些外部实体可以调用 Activity（请参阅 permission 属性）。

**android:permission**

客户端启动 Activity 或以其他方式令其响应 Intent 而必须具备的权限的名称。 如果系统尚未向 startActivity() 或 startActivityForResult() 的调用方授予指定权限，其 Intent 将不会传递给 Activity。

如果未设置该属性，则对 Activity 应用 <application> 元素的 permission 属性设置的权限。 如果这两个属性均未设置，则 Activity 不受权限保护。

**android:multiprocess**

是否可以将 Activity 实例启动到启动该实例的组件进程内 —“true”表示可以，“false”表示不可以。默认值为“false”。

正常情况下，新的 Activity 实例会启动到定义它的应用进程内，因此所有 Activity 实例都在同一进程内运行。 不过，如果该标志设置为“true”，Activity 实例便可在多个进程内运行，这样系统就能在任何使用实例的地方创建实例（前提是权限允许这样做），但这几乎毫无必要性或可取之处。

**android:process**

应在其中运行 Activity 的进程的名称。正常情况下，应用的所有组件都在为应用创建的默认进程名称内运行，您无需使用该属性。但在必要时，您可以使用该属性替换默认进程名称，以便让应用组件散布到多个进程中。

如果为该属性分配的名称以冒号（“:”）开头，则会在需要时创建应用专用的新进程，并且 Activity 会在该进程中运行。如果进程名称以小写字符开头，Activity 将在该名称的全局进程中运行，前提是它拥有相应的权限。这可以让不同应用中的组件共享一个进程，从而减少资源占用。

<application> 元素的 process 属性可为所有组件设置一个不同的默认进程名称。

## 参考

- [Android Manifest Activity Element](https://developer.android.com/guide/topics/manifest/activity-element.html)