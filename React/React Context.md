# React Context

Context 设计的目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。

## 创建

使用**React.createContext()**创建一个Context对象。

```jsx
const MyContext = React.createContext([defaultValue])
// defaultValue 为提供的默认值
```

## 使用

在**[ContextName].Provider**中使用自定义的Context，使用Context可以避免通过中间元素传递`props`：

```jsx
<MyContext.Provider value={someValue}>
	{/* ... */}
</MyContext.Provider>
```

**在class组件内使用：**

```jsx
class Child extends Component {
    // 指定读取的context
	static contextType = MyContext;
	render(){
        // 使用 this.context 访问 Provider 提供的值
    	return(
    		<div>{this.context}</div>
    	)
	}
}
```

**在函数式组件内使用：**

```jsx
function Child(){
    return (
    	<MyContext.Consumer>
        	{value => {
                return (
                	<h1>this is the value: {value}</h1>
                )
            }}
        </MyContext.Consumer>
    )
}
```

只有当组件所在的树中没有匹配到`Provider`时，其`defaultValue`才会生效。

**注意：**当祖先组件 provide 的值发生更新时，使用了该 provide 的值的子组件也会进行页面更新，此时子组件的更新**不受**其父组件的`shouldComponentUpdate()`生命周期函数的影响（即使该生命周期返回了false，页面也会进行相应更新，只不过不会调用子组件的 render 方法）。

### contextType 属性

该属性为挂载在组件class上的静态属性之一，值应为一个由`React.createContext()`创建的Context对象，从而我们能够使用`this.context`来获取到最近且匹配的Context上的那个值，可以在组件内的任何位置访问`this.context`。

如果是函数式组件在使用Context，则不用管这个属性。

### 使用多个Context

```jsx
// 提供时
<OneContext.Provide value={oneValue}>
	<AnotherContext.Provide value={anotherValue}>
    	<OneComponent />
    </AnotherContext.Provide>
</OneContext.Provide>

// 使用时
<OneContext.Consumer>
	{oneValue => (
    	<AnotherContext.Consumer>
        	{anotherValue => (
            	<AnotherComponent>
                	{oneValue}
                    {anotherValue}
                </AnotherComponent>
            )}
        </AnotherContext.Consumer>
    )}
</OneContext.Consumer>
```

如果两个或者更多的 context 值经常被一起使用，那你可能要考虑一下另外创建你自己的渲染组件，以提供这些值。

### 使用之前的注意事项

Context主要应用于很多不同层级组件需要访问同样一些的数据，使用时需要谨慎，因为这会使得组件的复用性变差。

一种无需Context的解决办法是将子组件自身通过props传递下去，因此中间组件无需知道子组件需要的是什么数据，但是这种将逻辑提升到组件树的更高层次来处理，会使得这些高层组件变得更复杂，并且会强行将低层组件适应这样的形式，这可能不会是我们想要的结果。

## 参考资料

+ [React Context](https://zh-hans.reactjs.org/docs/context.html)