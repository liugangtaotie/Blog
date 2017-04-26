## 定义

Annotation是一种应用于类、方法、参数、变量、构造器及包声明中的特殊修饰符。它是一种用来描述元数据的数据。

本质上，Annotation是一种特殊的接口，程序可以通过反射来获取指定程序元素的Annotation对象，然后通过Annotation对象来获取注解里面的元数据。

## 功能

- **编写文档**：通过代码里标识的元数据生成文档。【生成文档doc文档】
- **代码分析**：通过代码里标识的元数据对代码进行分析。【使用反射】
- **编译检查**：通过代码里标识的元数据让编译器能够实现基本的编译检查。【Override】

## API

### Annotation(元注解)

|类型|定义|
|---|---|
|Documented|含有该注解类型的元素(带有注释的)会通过javadoc或类似工具进行文档化|
|Inherited|表示注解类型能被自动继承|
|Retention|表示注解类型的存活时长|
|Target|表示注解类型所适用的程序元素的种类|

### Enum(枚举)

|类型|定义|
|---|---|
|ElementType|用于Target注解类型，程序元素类型|
|RetentionPolicy|用于Retention注解类型，注解保留策略|

### Exception(异常)

|类型|定义|
|---|---|
|AnnotationTypeMismatchException|当注解经过编译(或序列化)后，注解类型不匹配异常|
|IncompleteAnnotationException|当注解经过编译(或序列化)后，注解不完整异常|
|AnnotationFormatError|注解格式化错误|

## 系统自带

### 注解列表

|类型|定义|
|---|---|
|@Deprecated|已过时，不建议使用|
|@Override|覆盖父类中的方法|
|@Documented|所标注内容，可以出现在javadoc中|
|@Inherited|标注Annotation类型，用来表示Annotation具有继承性|
|@Target|标注Annotation类型，用来指定Annotation的ElementType属性。
|@Retention|标注Annotation类型，用来指定Annotation的RetentionPolicy属性|
|@SuppressWarnings|忽略编译器发出的警告|

### ElementType

```
public enum ElementType {
     /* 类、接口（包括注释类型）或枚举声明 */
    TYPE,
    /* 字段声明（包括枚举常量）*/
    FIELD,
    /* 方法声明 */
    METHOD,
    /* 参数声明 */
    PARAMETER,
    /* 构造方法声明 */
    CONSTRUCTOR,
    /* 局部变量声明 */
    LOCAL_VARIABLE,
    /* 注释类型声明 */
    ANNOTATION_TYPE,
    /* 包声明 */
    PACKAGE,
    /* JDK 1.8 以后参数声明 */
    TYPE_PARAMETER,
    /* JDK 1.8 以后类型声明 */
    TYPE_USE
}
```

### RetentionPolicy

```
public enum RetentionPolicy {
    /* Annotation信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了 */
    SOURCE,
    /* 编译器将Annotation存储于类对应的.class文件中。默认行为 */
    CLASS,
    /* 编译器将Annotation存储于class文件中，并且可通过反射机制读取。(java.lang.reflect.AnnotatedElement) */
    RUNTIME
}
```

### SuppressWarnings

```
all	         to suppress all warnings
boxing 	     to suppress warnings relative to boxing/unboxing operations
cast	     to suppress warnings relative to cast operations
dep-ann	     to suppress warnings relative to deprecated annotation
deprecation	 to suppress warnings relative to deprecation
fallthrough	 to suppress warnings relative to missing breaks in switch statements
finally 	 to suppress warnings relative to finally block that don’t return
hiding	     to suppress warnings relative to locals that hide variable
nls	         to suppress warnings relative to non-nls string literals
null	     to suppress warnings relative to null analysis
rawtypes	 to suppress warnings relative to un-specific types when using generics on class params
restriction	 to suppress warnings relative to usage of discouraged or forbidden references
serial	     to suppress warnings relative to missing serialVersionUID field for a serializable class
unchecked	 to suppress warnings relative to unchecked operations
unused	     to suppress warnings relative to unused code
static-access        to suppress warnings relative to incorrect static access
synthetic-access     to suppress warnings relative to unoptimized access from inner classes
incomplete-switch	 to suppress warnings relative to missing entries in a switch statement (enum case)
unqualified-field-access	to suppress warnings relative to field access unqualified
```

## 自定义

Android最常见的两行代码莫过于`.findViewById();`及`.setOnClickListener()`了。

下面我们就以这两个为例分别自定义`Bind`和`Click`注解

### Bind

**定义**：变量注解

```
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Bind {
    int id();
}
```

**反射**

```
public class BindUtil {

    /**
     * 初始化
     *
     * @param currentClassObject 变量中有 Bind 注解的那个对象
     * @param rootView           变量中有 Bind 注解的那个View
     */
    public static void initView(Object currentClassObject, View rootView) {
        Field[] fields = currentClassObject.getClass().getDeclaredFields();// 获取所有成员变量
        if (fields != null && fields.length > 0) {
            for (Field field : fields) {
                Bind bind = field.getAnnotation(Bind.class);
                if (bind != null) {
                    int viewId = bind.id();
                    field.setAccessible(true);//私有成员反射
                    try {
                        // 给currentClassObject中的变量field赋值
                        field.set(currentClassObject, rootView.findViewById(viewId));
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }
        }
    }

    /**
     * @param activity 使用 Bind 注解的那个变量
     */
    public static void initView(Activity activity) {
        initView(activity, activity.getWindow().getDecorView());
    }
}
```

**注解处理器：使用**

```
public class MainActivity extends Activity {

    @Bind(id = R.id.btn)
    public Button btn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        BindUtil.initView(this);

        btn.setText("Bind注解");
    }
}
```

### Click

**定义**：方法注解

```
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface Click {
    int[] id() default -1;
}
```

**注解处理器：反射**

```
public class ClickUtil {

    /**
     * 初始化
     *
     * @param currentClassObject 变量中有 Click 注解的那个对象
     * @param rootView           变量中有 Click 注解的那个View
     */
    public static void initView(final Object currentClassObject, View rootView) {
        // 所有的监听放到一个方法中
        try {
            final Method method = currentClassObject.getClass().getDeclaredMethod("onClick", View.class);
            if (method != null) {
                Click click = method.getAnnotation(Click.class);
                if (click != null) {
                    int viewIds [] = click.id();
                    method.setAccessible(true);//私有成员反射

                    for (int id : viewIds) {
                        View view = rootView.findViewById(id);
                        view.setOnClickListener(new View.OnClickListener() {
                            @Override
                            public void onClick(View v) {
                                try {
                                    method.invoke(currentClassObject, v);
                                } catch (Exception e) {
                                    e.printStackTrace();
                                }
                            }
                        });
                    }
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * @param activity 使用 Click 注解的那个变量
     */
    public static void initView(Activity activity) {
        initView(activity, activity.getWindow().getDecorView());
    }
}
```

**使用**

```
public class MainActivity extends Activity {

    @Bind(id = R.id.btn)
    public Button btn;

    @Bind(id = R.id.btn2)
    public Button btn2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        BindUtil.initView(this);

        ClickUtil.initView(this);

        btn.setText("Bind注解");
    }

    @Click(id = {R.id.btn, R.id.btn2})
    private void onClick(View view) {
        switch (view.getId()) {
            case R.id.btn:
                btn.setText("Click注解1");
                break;
            case R.id.btn2:
                btn2.setText("Click注解2");
                break;
        }
    }
}
```

### Click2

如果我们在使用的时候不想放在一个方法中，而是想一个方法绑定一个控件，那么也可以这样做。

**定义**:方法注解，数组变为单个

```
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface ClickSingle {
    int id() default -1;
}
```

**注解处理器：反射**

```
public class ClickUtilSingle {

    /**
     * 初始化
     *
     * @param currentClassObject 变量中有 Click 注解的那个对象
     * @param rootView           变量中有 Click 注解的那个View
     */
    public static void initView(final Object currentClassObject, View rootView) {
        // 所有的监听放到一个方法中
        try {
            final Method[] methods = currentClassObject.getClass().getDeclaredMethods();
            for (final Method method : methods) {
                ClickSingle click = method.getAnnotation(ClickSingle.class);
                if (click != null) {
                    int viewId = click.id();
                    method.setAccessible(true);//私有成员反射
                    final View view = rootView.findViewById(viewId);
                    view.setOnClickListener(new View.OnClickListener() {
                        @Override
                        public void onClick(View v) {
                            try { // 反射执行某个方法
                                method.invoke(currentClassObject, view);
                            } catch (Exception e) {
                                e.printStackTrace();
                            }
                        }
                    });
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * @param activity 使用 Click 注解的那个变量
     */
    public static void initView(Activity activity) {
        initView(activity, activity.getWindow().getDecorView());
    }
}
```

**使用**

```
public class MainActivitySingle extends Activity {

    @Bind(id = R.id.btn)
    public Button btn;

    @Bind(id = R.id.btn2)
    public Button btn2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        BindUtil.initView(this);

        ClickUtilSingle.initView(this);

        btn.setText("Bind注解");
    }

    @ClickSingle(id = R.id.btn)
    private void click1(View view) {
        btn.setText("ClickSingle注解1");
    }

    @ClickSingle(id = R.id.btn2)
    private void click2(View view) {
        btn2.setText("ClickSingle注解2");
    }
}
```

### 其他

上面降到了控件绑定和反射的关系，但是我们不推荐各种反射绑定组件，因为反射太耗时，我们只需知道原理即可。

市面上常用的 [ButterKnife](https://github.com/JakeWharton/butterknife) 的注解处理器的原理并不是反射机制，而是在编译期自动生成一个类似 XActivity$$ViewBinder 的类，去实现一堆堆的 findViewById 方法，性能跟手写 findViewById 差不多。

这里我们推荐使用 DataBinding ，性能超过 findViewById ，后续我会慢慢去剖析一些开源框架的源码。

## 更多

- 参考：《Java核心技术2》
- 源码：[DemoAnnotation](https://github.com/yuchao-wang/DemoAnnotation)