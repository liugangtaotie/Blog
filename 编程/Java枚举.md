### Java枚举

之前好像没有系统的整理过枚举的用法，最近发现枚举的用法还是蛮灵活的，重新学习记录一下。

### 常量

```
public enum Color {
    RED, BLACK, WHITE
}
```

### 实体类
```
public enum Color {
    BLACK("黑色", 0x000000),
    WHITE("白色", 0xFFFFFF),
    RED("红色", 0xFF0000);

    private String name;
    private int hexadecimal;

    Color(String name, int hexadecimal) {
        this.name = name;
        this.hexadecimal = hexadecimal;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getHexadecimal() {
        return hexadecimal;
    }

    public void setHexadecimal(int hexadecimal) {
        this.hexadecimal = hexadecimal;
    }

    @Override
    public String toString() {
        return "Color{" +
                "name='" + name + '\'' +
                ", hexadecimal=" + hexadecimal +
                '}';
    }

    public static void show(Color color, ColorInterface colorInterface) {
        switch (color) {
            case BLACK:
                colorInterface.showBlack();
                break;
            case WHITE:
                colorInterface.showWhite();
                break;
            case RED:
                colorInterface.showToRed();
                break;
        }
    }
}
```

**使用接口达到复用的效果**

```
public interface ColorInterface {
    void showBlack();
    void showWhite();
    void showToRed();
}
```

### 抽象类

```
public enum Color {
    BLACK {
        @Override
        public String getName() {
            return "黑色";
        }

        @Override
        public int getHexadecimal() {
            return 0x000000;
        }
    },
    WHITE {
        @Override
        public String getName() {
            return "白色";
        }

        @Override
        public int getHexadecimal() {
            return 0xFFFFFF;
        }
    },
    RED {
        @Override
        public String getName() {
            return "红色";
        }

        @Override
        public int getHexadecimal() {
            return 0xFF0000;
        }
    };

    public abstract String getName();

    public abstract int getHexadecimal();
}
```

**注意：就我而言，最常用的情景是接口返回了某一种数据类型，自动转换为其中的一种枚举对象。那么我们就要用到注解和反射了**

### 参考

[博客园](http://www.cnblogs.com/linjiqin/archive/2011/02/11/1951632.html) : http://www.cnblogs.com/linjiqin/archive/2011/02/11/1951632.html