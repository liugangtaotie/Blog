## 生命周期

![生命周期](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-fragment-01.jpg)

![对比](https://wangyuchao.oss-cn-beijing.aliyuncs.com/blog/program/android-fragment-02.jpg)

## FragmentTransaction

**add()**
```
onAttach()
onCreate()
onCreateView()
onActivityCreate()
onStart()
onResume()
```

**remove()**
```
onPause()
onStop()
onDestroyView()
onDestroy()
onDetach()
```
**show()和hide()**:不影响生命周期，只是显示和隐藏

**attach()**
```
onCreateView()
onActivityCreate()
onStart()
onResume()
```

**detach()**
```
onPause()
onStop()
onDestroyView()
```

**replace():FragmentA->FragmentB**
```
如果A压栈：
FragmentA:onPause()->onStop()->onDestroyView()
如果A未压栈：完全销毁
FragmentA:onPause()->onStop()->onDestroyView()->onDestroy()->onDetach()
FragmentB:onAttach()->...->onResume()
```

## 未完待续