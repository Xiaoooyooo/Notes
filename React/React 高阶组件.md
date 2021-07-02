# React 高阶组件

一言概之，高阶组件是参数为组件，返回值为新组件的**函数**。

它是React中用于复用组件逻辑的一种高级技巧，是一种基于React的组合特性而形成的设计模式。

```jsx
function EnhancedComponent(OneComponent){
    class NewComponent extends Component {
        /* ... */
        render() {
            return (
            	<OneComponent>
                	{ /* ... */}
                </OneComponent>
            )
        }
    }
    return NewComponent;
}
```

## 使用高阶组件的注意事项

### 不要改变原始组件，而是使用组合

```jsx
function EhancedComponent (OneComponent) {
    OneComponent.prototype.componentDidUpdate = function() {
        /* ... */
    }
    return OneComponent;
}
```

这样做会产生一些不良后果：

+ 组件无法再像增强之前一样使用了
+ 在别处的同样的增强操作会覆盖之前的增强操作
+ 如果该HOC增强了组件的生命周期函数，则该HOC不能应用于没有生命周期的函数组件

同时HOC也不应该修改传入的组件，而是要使用组合的方式，通过将组件包装在容器中实现功能：

```jsx
function EnhancedComponent(OneComponent){
    return class NewComponent extends Component {
        componentDidUpdate() {
        /* ... */
    	}
    	render() {
        	// 将组件包装在容器中而不直接进行修改
        	return (
        		<OneComponent></OneComponent>
        	)
    	}
    }
}
```

### 约定：将不相关的props传递给被包裹的组件

HOC为被包裹组件添加特性，其返回的组件应该与原组件保持类似的接口。

HOC应该透传与自身无关的props，大多数HOC都应该包含一个类似于下面的render方法：

```jsx
render() {
    const { extraProp, ...passThroughProps } = this.props;
    
    // 将props注入到被包装的组件中
    // 通常为state的值或实例方法
    const injectedProp = someStateOrInstanceMethod;
    
    // 将props传递给被包裹组件
    return (
    	<OneComponent 
         injectedProp={injectedProp} 
         {...passThroughProps}
        >
        </OneComponent>
    )
}
```

### 约定：最大化可组合性

详见[约定：最大化可组合性](https://zh-hans.reactjs.org/docs/higher-order-components.html#convention-maximizing-composability)

### 约定：包装显示名称以便轻松调试

```jsx
function EnhancedComponent(OneComponent) {
    class NewComponent extends Component {
        render() {
            return (
            	<OneCOmponent></OneCOmponent>
            )
        }
    }
    // 修改/添加 displayName
    NewComponent.displayName = "ANewNameForNewComponent";
    return NewComponent;
}
```

### 注意：不要再render方法中使用HOC

### 注意：务必复制静态方法

有时定义在被包裹组件上的静态方法很有用，当在应用HOC时，原始组件将被容器组件包装，因而新组件没有原始组件的任何方法。

```jsx
function EnhancedComponent(OneCOmponent){
    class NewComponent extends COmponent {
        /* ... */
    }
    NewComponent.oneStaticMethod = OneComponent.oneStaticMethod;
    return NewComponent;
}
```

### 注意：refs不会被传递

解决办法，使用`React.forwardRef`

## 参考资料

+ [React 高阶组件](https://zh-hans.reactjs.org/docs/higher-order-components.html)