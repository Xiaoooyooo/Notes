---
title: React组合
date: 2021-03-22 18:39:36
tags:
	- 前端框架
	- React
categories:
	- React
---

# React组合

React有着十分强大的组合模式，推荐使用组合而非继承来实现组件间的代码复用。

## 包含关系

有的组件在定义时无法得知将来它的子组件会是什么，如`Sidebar`侧边栏和`Dialog`对话框等展现通用容器的组件特别容易遇到这种情况。

我们可以在这些组件中使用一个特殊的`children` prop来将它们的子组件传递到渲染结果中：

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

这使得别的组件可以通过JSX嵌套，将任意组件作为子组件传递给它们：

```jsx
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

`<FancyBorder>` JSX 标签中的所有内容都会作为一个 `children` prop 传递给 `FancyBorder` 组件。因为 `FancyBorder` 将 `{props.children}` 渲染在一个 `<div>` 中，被传递的这些子组件最终都会出现在输出结果中。

在某些情况下，我们可以在一个组件中预留出几个位置，这时，我们可以不用`children`，而是自行约定：将所需内容传入porps，并使用相应的prop：

```jsx
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane left={<Contacts />} right={<Chat />} />
  );
}
```

`<Contacts />` 和 `<Chat />` 之类的 React 元素本质就是对象（object），所以你可以把它们当作 props，像其他数据一样传递。这种方法可能使你想起别的库中“槽”（slot）的概念，但在 React 中没有“槽”这一概念的限制，**你可以将任何东西作为 props 进行传递**。

## 特例关系

有些时候，我们会把一些组件看作是其他组件的特殊实例，比如 `WelcomeDialog` 可以说是 `Dialog` 的特殊实例。在 React 中，我们也可以通过组合来实现这一点。“特殊”组件可以通过 props 定制并渲染“一般”组件：

```jsx
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

组合也同样适用于以 class 形式定义的组件。