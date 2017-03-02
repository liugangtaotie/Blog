### 成员内部类
 **Test$TestInner.class**
 - 可以直接使用外部类的所有成员和方法
 - 外部类中必须创建一个成员内部类的对象才能访问内部类
 ```
 public class Test {
     class TestInner {
     }
 }
 // 创建
 Test.TestInner testInner = new Test().new TestInner();
 ```

### 静态内部类
**Test$TestInner.class**
- 不需要依赖外部类
- 不能使用外部类的非static成员变量和方法
 ```
 public class Test {
     static class TestInner {
     }
 }
 // 创建
 Test.TestInner testInner = new Test.TestInner();
 ```

### 局部内部类
**Test$1TestInner.class**
- 可以在某个方法中
- 也可以在某一个作用域中
 ```
 public class Test {
     public void test(){
         class TestInner{
             public void testInner() {
             }
         }
         new TestInner().testInner();
     }
 }
 ```

### 匿名内部类
- 主要用来进行接口回调
**MainClass$1.class**
 ```
   button.setOnClickListener(new OnClickListener() {
        @Override
        public void onClick (View v){
        }
    });
 ```

### 优点
 - 代码整洁可读性高
 - 对其他外部类隐藏
 - 便捷的回调