---
title: 了解JSX
date: 2021-03-21 17:03:14
tags:
	- JavaScript
	- React
	- JSX
categories:
	- React
---

# 了解JSX

考虑以下变量声明：

```jsx
const ele = <h1>Hello, World!</h1>
```

这个有趣的标签语法既不是字符串也不是HTML，它就是JSX，是一个JS的语法扩展。

在React中配合JSX能够很好地描述UI应该呈现出它应有交互的本质形式。

JSX还可以生成React元素。

## 在JSX中嵌入表达式

当使用JSX时，我们可以在大括号`{}`中嵌入表达式（An *expression* is any valid unit of code that resolves to a value，即任何能够解析为值的有效代码单元），包括一个变量，返回其他值的函数或三元表达式等。

```jsx
const name = 'Tom'
const element = <div>Hi, I'm {name}</div>
const element1 = <div>{()=>name}</div>
```

为了便于阅读，我们可以将JSX代码拆为多行，同时为了避免遇到自动插入分号陷阱，我们可以将多行的JSX代码用圆括号包裹起来。

```jsx
const element = (
	<div>
    	<h1>Hello World</h1>
    </div>
)
```

## JSX也是一个表达式

这意味着我们可以在 `if` 语句和 `for` 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX，如下：

```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

## JSX特定属性

可以使用引号来将属性值指定为字符串字面量，

也可以使用大括号在属性值中插入一个JS表达式：

```jsx
const element = <div tabIndex="0"></div>;
const element = <img src={user.avatarUrl}></img>;
```

在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。你应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，对于同一属性不能同时使用这两种符号。

因为JSX在语法上更像JS而不是HTML，所以React DOM使用小驼峰来定义属性，如`class`变为`className`，`tabindex`变为`tabIndex`

## 使用JSX指定子元素

如果标签内没有内容，则可以使用`/>`闭合标签，如`<myComponent />`。

JSX标签中能够包含很多子元素：

```jsx
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

## JSX防止注入攻击

我们可以安全地在JSX中插入用户的输入内容，React DOM在渲染所有输入内容之前会默认进行转义，它可以确保我们的应用中永远不会注入那些并非自己所明确编写的内容，这样可以有效防止[XSS（cross-site-scripting, 跨站脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。

## JSX表示对象

Babel会把JSX转译为一个名为`React.createElement()`函数调用，以下两种写法完全等效：

```jsx
//写法一
const element = (
	<h1 className="Greeting">
    	Hello World!
    </h1>
)
//写法二
const element = React.createElement("h1",{
    className: "greeting"
}, "Hello World!")
```

`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：

```jsx
// 注意：这是简化过的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

## 参考资料

+ [JSX简介](https://react.docschina.org/docs/introducing-jsx.html)

