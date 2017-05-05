## 设计模式
设计模式（Design pattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。 毫无疑问，设计模式于己于他人于系统都是多赢的；设计模式使代码编写真正工程化；设计模式是软件工程的基石脉络，如同大厦的结构一样。

### 原则
- 单一职责
- 里氏替换：子类可以扩展父类的功能，但不能改变父类原有的功能
- 依赖倒置：面向接口编程
- 接口隔离：一个类对另一个类的依赖应该建立在最小的接口上
- 迪米特法则： 降低耦合度，只与直接的朋友通信
- 开闭原则：对扩展开放，对修改关闭。用抽象构建框架，用实现扩展细节

### 简单工厂
- 选择实现

```
public interface Api {
    public void operation(String s);
}

public class ImplA implements Api {
    @Override
    public void operation(String s) {
    }
}

public class ImplB implements Api {
    @Override
    public void operation(String s) {
    }
}

public class Factory {
    public static Api createApi(int condition) {
        Api api = null;
        if (condition == 1) {
            api = new ImplA();
        } else if (condition == 2) {
            api = new ImplB();
        }
        return api;
    }
}

public class Client {
    public static void main(String[] args) {
        Api api = Factory.createApi(1);
        api.operation("正在创建简单工厂");
    }
}
```

### 外观模式
- 封装交互，简化调用

```
public class Facade {
    public void test() {
        AModuleApi a = new AModuleImpl();
        a.testA();
        BModuleApi b = new BModuleImpl();
        b.testB();
        CModuleApi c = new CModuleImpl();
        c.testC();
    }
}

public class Client {
    public static void main(String[] args) {
        new Facade().test();
    }
}
```

### 适配器模式
- 匹配转换，复用功能

```
    public class Adaptee {
        public void originalMethod() {
        }
    }

    public interface Target {
        public void method();
    }

    //对象适配器
    public class Adapter1 implements Target {
        private Adaptee adaptee;

        public Adapter1(Adaptee adaptee) {
            this.adaptee = adaptee;
        }

        @Override
        public void method() {
            adaptee.originalMethod();
        }
    }

    //类适配器
    public class Adapter2 extends Adaptee implements Target {
        @Override
        public void method() {
            super.originalMethod();
        }
    }
```

### 单例模式

```
    public class Singleton {
        private static Singleton instance = null;

        private Singleton() {
        }

        public synchronized static Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
    }
```

### 建造者模式

```
    public interface Builder {
        public void buildPartA();

        public void buildPartB();

        public void buildPartC();

        Product getResult();
    }

    public interface Product {
    }

    //具体建造者
    public class ConcreateBuilder implements Builder {
        @Override
        public void buildPartA() {
        }

        @Override
        public void buildPartB() {
        }

        @Override
        public void buildPartC() {
        }

        @Override
        public Product getResult() {
            return null;
        }
    }

    //指导者
    public class Director {
        private Builder builder;

        public Director(Builder builder) {
            this.builder = builder;
        }

        public void buildAll() {
            builder.buildPartA();
            builder.buildPartB();
            builder.buildPartC();
        }
    }

    public void client() {
        ConcreateBuilder builder = new ConcreateBuilder();
        Director director = new Director(builder);
        director.buildAll();
        Product product = builder.getResult();
    }
```

### 中介者模式
- 根据名字显而易见，作为两个对象的中介，A,B,C,D,E互相不认识，但是需要互相联系，因此只要认识中介X就行了，也就是由网状图结构变为星射线结构。

```
//同事抽象类
public abstract class Colleague {
    private Mediator mediator;

    public Colleague(Mediator mediator) {
        this.mediator = mediator;
    }

    public Mediator getMediator() {
        return mediator;
    }
}
//具体同事类
public class ConcreteColleagueA extends Colleague {
    public ConcreteColleagueA(Mediator mediator) {
        super(mediator);
    }

    public void someOperationA() {
        getMediator().changed(this);
    }
}
public class ConcreteColleagueB extends Colleague {
    public ConcreteColleagueB(Mediator mediator) {
        super(mediator);
    }

    public void someOperationB() {
        getMediator().changed(this);
    }
}
public class ConcreteColleagueC extends Colleague {
    public ConcreteColleagueC(Mediator mediator) {
        super(mediator);
    }

    public void someOperationC() {
        getMediator().changed(this);
    }
}
//中介接口
public interface Mediator {
    public void changed(Colleague colleague);
}
//具体中介
public class ConcreteMediator implements Mediator {

    private ConcreteColleagueA colleagueA;
    private ConcreteColleagueB colleagueB;
    private ConcreteColleagueC colleagueC;

    public void setConcreteColleagueA(ConcreteColleagueA colleague) {
        this.colleagueA = colleague;
    }

    public void setConcreteColleagueB(ConcreteColleagueB colleague) {
        this.colleagueB = colleague;
    }

    public void setConcreteColleagueC(ConcreteColleagueC colleagueC) {
        this.colleagueC = colleagueC;
    }

    /**
     * 统一由中介处理关系同事类之间的关系
     */
    public void changed(Colleague colleague) {
        if (colleague == colleagueA) {
            colleagueB.someOperationB();
            colleagueC.someOperationC();
        } else if (colleague == colleagueB) {
            colleagueA.someOperationA();
            colleagueC.someOperationC();
        } else if (colleague == colleagueC) {
            colleagueA.someOperationA();
            colleagueB.someOperationB();
        }
    }
}
	//客户端实现
    public void client(){
        ConcreteMediator mediator = new ConcreteMediator();

        ConcreteColleagueA colleagueA = new ConcreteColleagueA(mediator);
        ConcreteColleagueB colleagueB = new ConcreteColleagueB(mediator);
        ConcreteColleagueC colleagueC = new ConcreteColleagueC(mediator);

        mediator.setConcreteColleagueA(colleagueA);
        mediator.setConcreteColleagueB(colleagueB);
        mediator.setConcreteColleagueC(colleagueC);

        colleagueA.someOperationA();
    }
```

### 观察者模式
- 动态联动，广播通信

```
    public interface Observer {
        public void update(Subject subject);
    }

    public class ConcreteObserver implements Observer {
        private String observerState;

        public void update(Subject subject) {
            observerState = ((ConcreteSubject) subject).getSubjectState();
        }
    }

    public class Subject {
        private ArrayList<Observer> observers = new ArrayList<Observer>();

        public void attach(Observer observer) {
            observers.add(observer);
        }

        public void detach(Observer observer) {
            observers.remove(observer);
        }

        protected void notifyObservers() {
            for (Observer observer : observers) {
                observer.update(this);
            }
        }
    }

    public class ConcreteSubject extends Subject {
        private String subjectState;

        public String getSubjectState() {
            return subjectState;
        }

        public void setSubjectState(String subjectState) {
            this.subjectState = subjectState;
            this.notifyObservers();
        }
    }

    public void client() {
        //创建被观察者
        ConcreteSubject subject = new ConcreteSubject();
        //创建观察者
        ConcreteObserver observer1 = new ConcreteObserver();
        ConcreteObserver observer2 = new ConcreteObserver();
        //添加观察着
        subject.attach(observer1);
        subject.attach(observer2);
        //被观察者调用方法
        subject.setSubjectState("大家伙过来领报纸了");
    }
```

### 代理模式
- 控制对象访问

```
//================普通代理================
    public interface Subject {
        public void operation();
    }

    public class RealSubject implements Subject {
        @Override
        public void operation() {
        }
    }

    public class Proxy implements Subject {
        private Subject subject;

        public Proxy() {
            this.subject = new RealSubject();
        }

        public Proxy(Subject subject) {
            this.subject = subject;
        }

        @Override
        public void operation() {
            System.out.println("before");
            this.subject.operation();
            System.out.println("after");
        }
    }

    public void client() {
        Subject realSubject = new RealSubject();
        Proxy proxy = new Proxy(realSubject);
        proxy.operation();
    }   
//================动态代理================ 
    public interface Subject {
        public void operation();
    }

    public class RealSubject implements Subject {
        @Override
        public void operation() {
        }
    }

    public class ProxyHandler implements InvocationHandler {
        private Object proxied;

        public ProxyHandler(Object proxied) {
            this.proxied = proxied;
        }

        @Override
        public Object invoke(Object o, Method method, Object[] objects) throws Throwable {
            System.out.println("before");
            Object object = method.invoke(proxied, objects)
            System.out.println("after");
            return object;
        }
    }


    public void client() {
        Subject realSubject = new RealSubject();
        ProxyHandler handler = new ProxyHandler(realSubject);
        Subject proxy = (Subject) Proxy.newProxyInstance(
                realSubject.getClass().getClassLoader(),
                realSubject.getClass().getInterfaces(),
                handler);
        proxy.operation();
    }
```

### 命令模式
- 封装请求

```
    public interface Command {
        public void execute();
    }

    //接收者
    public class Receiver {
        public void action() {
            System.out.println("真正执行命令操作的代码");
        }
    }

    //具体命令实现对象
    public class ConcreteCommand implements Command {
        private Receiver receiver;

        public ConcreteCommand(Receiver receiver) {
            this.receiver = receiver;
        }

        @Override
        public void execute() {
            receiver.action();
        }
    }

    //调用者
    public class Invoker {
        private Command command = null;

        public void setCommand(Command command) {
            this.command = command;
        }

        public void runCommand() {
            command.execute();
        }
    }

    public void client() {
        //创建命令，并设定接收者
        Receiver receiver = new Receiver();
        Command command = new ConcreteCommand(receiver);
        //设置调用对象
        Invoker invoker = new Invoker();
        invoker.setCommand(command);
        //执行命令
        invoker.runCommand();
    }
```

### 组合模式
- 统一叶子对象和组合对象

```
    //组件类
    public abstract class Component {
        public abstract void someOperation();

        public void addChild(Component child) {
            throw new UnsupportedOperationException("叶子类没有此功能");
        }

        public void removeChild(Component child) {
            throw new UnsupportedOperationException("叶子类没有此功能");
        }

        public Component getChildren(int index) {
            throw new UnsupportedOperationException("叶子类没有此功能");
        }
    }

    //组合对象
    public class Composite extends Component {

        private List<Component> childComponents = null;

        @Override
        public void someOperation() {
            System.out.println("一些示例方法,比如说循环遍历");
        }

        @Override
        public void addChild(Component child) {
            if (childComponents == null) {
                childComponents = new ArrayList<Component>();
            }
            childComponents.add(child);
        }

        @Override
        public void removeChild(Component child) {
            if (childComponents != null) {
                childComponents.remove(child);
            }
        }

        @Override
        public Component getChildren(int index) {
            if (childComponents != null) {
                if (index > 0 && index < childComponents.size()) {
                    return childComponents.get(index);
                }
            }
            return null;
        }
    }

    //叶子类
    public class Leaf extends Component {
        @Override
        public void someOperation() {}
    }

    public void client() {
        Component root = new Composite();
        Component c1 = new Composite();
        Component c2 = new Composite();
        Component leaf1 = new Leaf();
        Component leaf2 = new Leaf();
        Component leaf3 = new Leaf();
        //树状结构
        root.addChild(c1);
        root.addChild(c2);
        root.addChild(leaf1);
        c1.addChild(leaf2);
        c2.addChild(leaf3);
        //一些操作...
        root.getChildren(0);
    }
```

### 模板方法模式
- 固定算法骨架

```
    public abstract class Templete {
        public void beforeOperation() {}

        public abstract void operation();

        public void afterOperation() {}

        public void method() {
            beforeOperation();
            operation();
            afterOperation();
        }
    }

    public class ConcreteTemplete extends Templete {
        @Override
        public void operation() {
            System.out.println("具体操作");
        }
    }

    public void client() {
        Templete concreteTemplete = new ConcreteTemplete();
        concreteTemplete.method();
    }
```

### 策略模式
- 分离算法，选择实现
- 最典型的例子就是：根据会员的级别，进行不同程度的折扣

```
    public class Context {
        private Strategy strategy;

        public Context(Strategy strategy) {
            this.strategy = strategy;
        }

        public void contetextMethod() {
            this.strategy.method();
        }
    }

    public interface Strategy {
        public void method();
    }

    public class ConcreteStrategyA implements Strategy {
        @Override
        public void method() {
            System.out.println("A算法");
        }
    }

    public class ConcreteStrategyB implements Strategy {
        @Override
        public void method() {
            System.out.println("B算法");
        }
    }

    public class ConcreteStrategyC implements Strategy {
        @Override
        public void method() {
            System.out.println("C算法");
        }
    }

    public void client() {
        //这里对象可以是ABC中任意一个
        ConcreteStrategyA concreteStrategyA = new ConcreteStrategyA();
        Context context = new Context(concreteStrategyA);
        context.contetextMethod();
    }
```

### 装饰者模式
- 动态结合

```
    //组件对象
    public interface Component {
        public abstract void operation();
    }

    public class ConcreteComponent implements Component {
        @Override
        public void operation() {
            System.out.println("具体组件对象");
        }
    }

    //装饰者
    public abstract class Decorator implements Component {
        private Component component;

        protected Decorator(Component component) {
            this.component = component;
        }

        @Override
        public void operation() {
            System.out.println("装饰者原始方法...");
        }
    }

    //具体三个不同的装饰者A,B,C
    public class ConcreteDecoratorA extends Decorator {

        protected ConcreteDecoratorA(Component component) {
            super(component);
        }

        public void operationA() {
            System.out.println("装饰者A新的方法...");
        }

        @Override
        public void operation() {
            super.operation();
            operationA();
        }
    }

    public class ConcreteDecoratorB extends Decorator {

        protected ConcreteDecoratorB(Component component) {
            super(component);
        }

        public void operationB() {
            System.out.println("装饰者B新的方法...");
        }

        @Override
        public void operation() {
            super.operation();
            operationB();
        }
    }


    public class ConcreteDecoratorC extends Decorator {

        protected ConcreteDecoratorC(Component component) {
            super(component);
        }

        public void operationC() {
            System.out.println("装饰者C新的方法...");
        }

        @Override
        public void operation() {
            super.operation();
            operationC();
        }
    }

    //IO流就是装饰者模式
    public void client() {
        Component component = new ConcreteComponent();
        Decorator decorator = new ConcreteDecoratorC(
                new ConcreteDecoratorB(new ConcreteDecoratorA(component)));
        decorator.operation();
    }
```
