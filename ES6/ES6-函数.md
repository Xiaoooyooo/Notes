# ES6-函数

## 函数的默认值

变量的解构赋值

```js
//写法一,传入参数不一定触发所有默认值
function fn(url,{method:"GET",data:{}} = {}){
    //...
}

//写法二，传入参数一定不触发默认值
function fn(url,{method,data} = {method:"GET",data:{}}){
    //...
}
```

## 函数的length属性

返回没有指定默认值的参数个数。

```js
(function (a) {}).length // 1
(function (a = 5) {}).length // 0
(function (a, b, c = 5) {}).length // 2
```

这是因为`length`属性的含义是，该函数预期传入的参数个数。某个参数指定默认值以后，预期传入的参数个数就不包括这个参数了。函数的`length`属性，不包括 rest 参数。

## 作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域，等到初始化结束，这个作用域就会消失。这种语法行为，在不设置参数默认值时，是不会出现的。

```js
//利用参数默认值，可以指定某一个参数不能省略，如果省略则报错
function throwIfMissing(){
    throw new Error('Missing parameter')
}
function fn(a = throwIfMissing()){
    //...
}
fn()	//
```

可以将参数默认值设为`undefined`，表明这个参数是可以省略的。

## rest参数

ES6 引入 rest 参数（形式为`...变量名`），用于获取函数的多余参数，这样就不需要使用`arguments`对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

```js
function add(...values){
    return values.reduce((total,current) => total + current)
}
```

## 严格模式

只要参数使用了默认值、解构赋值、或者扩展运算符，就不能显式指定严格模式。
两种方法可以规避这种限制。第一种是设定全局性的严格模式，这是合法的。
第二种是把函数包在一个无参数的立即执行函数里面。

```js
'use strict';
function doSomething(a, b = a) {
  // code
}

//或者
const doSomething = (function () {
  'use strict';
  return function(value = 42) {
    return value;
  };
}());
```

## 部署管道机制

```js
let pipeline = (...fns) => val => fns.reduce((prev,current) => current(prev),val)
let add = a => a + 2
let mult = a => a * 3
pipeline(add,mult)(5)	//21
```

## 尾调用优化

尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指**某个函数的最后一步是调用另一个函数**。

```js
//尾调用优化示例
function f(x){
  return g(x);	//此时f(x)已经执行完毕，g(x)内部也没有依赖f(x)的变量，因此f(x)占用的内存已经被销毁。这样可以节约内存，防止发生栈溢出		
}

//以下三种情况都不属于尾调用
// 情况一，调用函数g之后，还有赋值操作，所以不属于尾调用
function f(x){
  let y = g(x);
  return y;
}

// 情况二，调用后还有操作，即使写在一行内
function f(x){
  return g(x) + 1;
}

// 情况三，相当于return undefined
function f(x){
  g(x);
}
```

**尾部调用优化之尾递归**

```js
//一个普通的递归求斐波那契数列
function Fibonacci(n) {
  if ( n <= 1 ) {return 1};
  return Fibonacci(n - 1) + Fibonacci(n - 2);	//这里调用了两次函数，并且需要做+操作，因此不算尾调用优化
}

//尾调用优化版
//目前只有 Safari 浏览器支持尾调用优化，Chrome 和 Firefox 都不支持
let Fibonacci = function(n,ac1 = 1,ac2 = 1){
    if(n <= 2) return ac2
    return Fibonacci(n - 1,ac2,ac1 + ac2)
}
```

尾递归的实现，往往需要改写递归函数，确保最后一步只调用自身。做到这一点的方法，就是**把所有用到的内部变量改写成函数的参数**。

```js
//尾调用优化版阶乘
let tailFactorial2 = function(n,total = 1){
        if(n == 1) return total
        return tailFactorial2(n - 1,total * n)
    }
```

**柯里化（currying）**

多参数的函数转换成单参数的形式。

