# Refs 和 DOM

Refs提供了一种方式，允许我们**访问**DOM节点或在render方法中创建的React元素。

**何时使用**

+ 管理焦点，文本选择或媒体播放
+ 触发强制动画
+ 集成第三方DOM库

**勿过度使用Refs**

让更高的组件层级拥有某项 state 来管理 app 中某些事情的发生是更恰当的。

## 创建Refs

React Refs 是使用`React.createRef()`创建的，并通过`ref`属性附加到 React 元素上，在构造组件时，通常将 Refs 分配给实例属性，以便可以在整个组件中引用它们。

```jsx
class MyComponent extends Component {
    constructor() {
        super();
        this.myRef = React.createRef();	// 创建Ref
    }
    render() {
        // 使用 ref 属性引用创建的 Ref 
        return (
        	<div ref={this.myRef}></div>
        )
    }
    // 其他位置使用 this.myRef.current 即可访问到该 div 的引用
}
```

## 访问Refs

当 ref 被传递给 render 中的元素时，对该节点的引用可以在 ref 的 `current` 属性中访问。

ref 的值根据节点的类型而有所不同：

+ HTML元素：底层的DOM元素
+ class组件：组件挂载的实例
+ **不能在函数组件上使用 ref 属性，因为它们没有实例**，但是可以在其内部使用 ref，只要它指向一个 DOM 元素或 class 组件

React 会在组件挂载时给 `current` 属性传入 DOM 元素或组件实例，并在组件卸载时传入 `null`，`ref`将在`componentDidMount`或`componentDidUpdate`生命周期钩子触发前更新，确保 ref 中始终保存的是最新的。

## 将DOM Refs暴露给父组件

详见**Ref 转发**

## 回调 Refs

该方式不用向`ref`属性传递`createRef()`创建的 Ref，而是选择传入一个回调函数，该回调函数默认会被传入一个参数，为当前 DOM 元素或 React 组件实例，然后我们可以在回调函数中保存该 DOM 元素或组件：

```jsx
class MyComponent extends Component {
    refCallback(element) {
        this.div = element;
    }
    render() {
        return (
        	<div ref={this.refCallback}></div>
        )
    }
}
```

这个回调函数可以是父组件传递的 prop，从而能够将子组件的 ref 保存在父组件中，但是该方式于**Ref 转发**不同。

**注意：**

[关于回调 refs 的说明](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html#caveats-with-callback-refs)

如果回调函数是通过内联函数的方式定义的，那么在更新过程中它会被执行两次，第一次传入参数`null`，第二次传入 DOM 元素。

这是因为每次渲染时都会创建一个新的函数实例，所以 React 清空旧的 ref，并设置新的。

通过将 ref 的回调函数定义为 class 的绑定函数可以避免上述问题，但是大多数情况下它是无关紧要的。

## 参考资料

+ [Refs & DOM](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html)