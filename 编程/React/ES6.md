## 变量

- let 局部变量 var 全局变量

## 模板字符串

- 使用 \`\` 包裹
- 可以有 html 标签 和 JS 代码
- 内部使用 `${}` 调用 JS 代码
- 可以嵌套

```
$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```

## 对象扩展

### 属性简介表示法

```
const foo = 'bar';
const baz = {foo};
baz // {foo: "bar"}

// 等同于
const baz = {foo: foo};
```

### 属性名表达式

```
let propKey = 'foo';

let obj = {
  [propKey]: true,
  ['a' + 'bc']: 123
};
```

### 常见方法

- call `aa.call(bb,cc);` 为了改变上下文this，this 指向 bb ，立刻调用 aa 方法，传入 cc 参数
- apply 改变上下文this，数组传递
- bind 返回函数，方便后续回调
- create 浅拷贝 ES5
- assign 为对象添加属性、方法、克隆对象（浅拷贝）、合并对象、为属性指定默认值。
- 拷贝最好使用 Json 互转
- Object.is() 是否相等

## 参考

- [阮一峰](http://es6.ruanyifeng.com/)