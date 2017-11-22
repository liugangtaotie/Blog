## HelloWorld

```
// 添加
npm install -g create-react-app

// 创建 不能有大写字母
create-react-app my-app

// 启动
cd my-app
npm start
```

## JSX

**写法** 使用 `{}` 传入支持 ES6 的 JS 代码
**JSX语法注释** `//` 会报警告问题。标准注释如下

```
{/**JSX标准注释**/}
```

### 事件绑定

由常见的 `onchange=""` 或 `onclick=""` 改为 `onChange={}` 或 `onClick={}` 

```
    render() {
        return (
            <div>
                <button onClick={this.handleClick}>
                    {/**这是注释：输出带括号**/}
                    {
                        `点击这里{${this.state.isToggleOn ? "开的状态" : "关的状态" }}`
                    }
                    {/**这是注释：输出不带括号**/}
                    点击这里{this.state.isToggleOn ? "开的状态" : "关的状态" }
                </button>
            </div>
        );
    }
```

## 生命周期

```
    // 构造方法
    constructor(props) {}
    // 渲染界面
    render() {}
    // 生命周期挂载
    constructor()
    componentWillMount()
    render()
    componentDidMount()
    // 更新组件触发
    componentWillReceiveProps();
    shouldComponentUpdate();
    componentWillUpdate();
    render();
    componentDidUpdate();
    // 生命周期卸载
    componentWillUnmount();
    // 异常捕获
    componentDidCatch()
    // 更新状态
    this.setState((prevState, props) => ({}));
    this.forceUpdate(callback);
    // 子组件更新父组件 
    this.props.onUpdate(); // 父组件需要定义 onUpdate
    // 类属性
    defaultProps
    displayName
    // 对象属性
    props
    state
```

## 参考

- [官网-HelloWorld](https://reactjs.org/docs/hello-world.html)
- [官网-生命周期](https://reactjs.org/docs/react-component.html#lifecycle-methods)
- [React中文教程](https://react.bootcss.com/react/)
- [阮一峰](http://www.ruanyifeng.com/blog/2015/03/react.html)