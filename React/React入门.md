---
title: React入门
date: 2021-03-20 12:07:25
tags:
	- React
categories:
	- React
---

# React入门

## 创建应用

```bash 
npx create-react-app demo01
```

## 基本概念

1. 组件声明

   ```jsx
   class newComponent extends React.component {
       constructor(props){
           //必须调用super
           super(props)
           this.state = {
               //用于存放当前组件的数据属性
               message: "Hello World"
           }
       }
       render(){
           return (
               <h1 class='test' onClick={()=>{
                       //修改当前组件的数据，使用setState()，而不能通过this.state.message的方式						  修改
                       this.setState({message:"Hello React"})
                   }}>{this.state.message}</h1>
           )
       }
   }
   ```

2. 一些术语

   + 受控组件：该组件的实例中没有state属性，其内部的数据全来自于父组件的props
   + 函数组件：如果一个组件内没有state属性，只包含一个render方法，那么我们可以将该组件定义为一个函数，这个函数接受props作为参数，然后返回需要渲染的元素。函数组件写起来不像class组件那么繁琐，许多组件都可以使用这种形式来写。

3. 一些问题及注意事项

   + 为什么不可变性在React中非常重要

     修改数据的方式：在原对象中直接修改，或替换原对象。后者的好处：

     + 简化复杂功能，使复杂的特性更加容易实现，如“时间旅行”功能，撤销和恢复
     + 跟踪数据的改变，如果在原数据上直接修改，那么我们可能需要进行遍历操作才能找出被修改的数据，而如果直接替换，则很容易就能发现原数据已被改变
     + 确定在React中何时重新渲染，不可变性最主要的优势在于它可以帮我们在React中创建[purecomponents](https://react.docschina.org/docs/optimizing-performance.html#examples)。我们可以很轻松的确定不可变数据是否发生了改变，从而确定何时对组件进行重新渲染。
     
   + 不能在render函数中修改state

