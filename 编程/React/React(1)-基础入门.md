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

## 事件绑定

由常见的 `onchange=""` 或 `onclick=""` 改为 `onChange={}` 或 `onClick={}` 

```
    render() {
        return (
            <div>
                <button onClick={this.handleClick}>
                    {/**这是注释：输出带括号（模板字符串）**/}
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

## ReactComponent 生命周期 

### 挂载(Mounting)

```
constructor(props)      // 构造方法
componentWillMount()    // 即将挂载
render()                // 渲染
componentDidMount()     // 挂载完成
```

### 更新(Updating)

```
componentWillReceiveProps(nextProps)        // 即将接收
shouldComponentUpdate(nextProps, nextState) // 组件是否应该更新
componentWillUpdate(nextProps, nextState)   // 组件即将更新
render();                                   // 组件渲染
componentDidUpdate(prevProps, prevState)    // 组件更新完成
```

#### shouldComponentUpdate(拦截器)

> Defaults to true. This method is not called for the initial render or when forceUpdate() is used.

**可以把它当做一个拦截器**

- true  默认为 `true`，这个方法在初始化渲染以及 `forceUpdate()` 时不会调用。
- false 当状态改变时，不会重新渲染 `componentWillUpdate()`，`render()`，`componentDidUpdate()`

#### setState（正常更新）

```
setState(updater[, callback])
setState((prevState, props) => ({}),callback);
```

#### forceUpdate（强制更新）

```
component.forceUpdate(callback);
```

> Calling forceUpdate() will cause render() to be called on the component, skipping shouldComponentUpdate(). This will trigger the normal lifecycle methods for child components, including the shouldComponentUpdate() method of each child.

`forceUpdate()` 方法会跳过 `shouldComponentUpdate()`  直接触发 `render()` 方法，会触发子组件正常的生命周期（包含 `shouldComponentUpdate` ）


### 卸载(Unmounting)

```
componentWillUnmount();
```

### 异常捕获(Error Handling)

```
componentDidCatch(error, info);
```

### 类属性

#### defaultProps

```
class CustomButton extends React.Component {
  // ...
}
// 默认属性
CustomButton.defaultProps = {
  color: 'blue'
};
```

#### displayName

> The displayName string is used in debugging messages.

### 对象属性

- props
- state

## ReactDOM

```
ReactDOM.render(element, container[, callback])     // 渲染
// Same as render(), but is used to hydrate a container whose HTML contents were rendered by ReactDOMServer.
ReactDOM.hydrate(element, container[, callback])    // 混合
// Remove a mounted React component from the DOM and clean up its event handlers and state
ReactDOM.unmountComponentAtNode(container)          // 移除container中的元素
// returns the corresponding native browser DOM element.
ReactDOM.findDOMNode(component)                     // 查找得到某个元素
// Creates a portal. Portals provide a way to render children into a DOM node that exists outside the hierarchy of the DOM component.
ReactDOM.createPortal(child, container)             // hierarchy 等级
```

## 参考

- [官网-HelloWorld](https://reactjs.org/docs/hello-world.html)
- [官网-生命周期](https://reactjs.org/docs/react-component.html#lifecycle-methods)
- [React中文教程](https://react.bootcss.com/react/)
- [阮一峰](http://www.ruanyifeng.com/blog/2015/03/react.html)