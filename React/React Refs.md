# Refs 转发

Ref 转发是一项将 ref 自动地通过组件传递到其一子组件的技巧。对于大多数应用来说不是必需的，但对于可重用的组件库等来说是很重要的。

Ref 转发是一个可选特性，其允许某些组件接收ref，并将其向下传递（转发）给子组件。

## 创建

使用`React.forwardRef()`创建的组件，在其内部存在一个`ref`参数，可以将该`ref`属性通过JSX属性绑定传递给子组件从而实现Ref的转发。

```jsx
const MyForwardRefCOmponent = React.forwardRef((props, ref) => {
    // 第二个参数 ref 仅在使用该方式定义组件时存在
    return (
        // 转发给一个组件
    	<AnotherComponent ref={ref}></AnotherComponent>
        // 或 DOM 元素
        <input ref={ref} />
    )
})
// 需要设置 displayName
MyForwardRefComponent.displayName = "MyForwardRefCOmponent"
```

在父组件中：

```jsx
/* ... */
constructor() {
    super();
    this.myRef = React.createRef();
}
/* ... */
render() {
    return (
    	<MyForwardRefCOmponent ref={this.myRef}></MyForwardRefCOmponent>
    )
}
```

