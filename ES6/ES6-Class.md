# ES6-Class

ES6 的`class`可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

ES6 的类，完全可以看作构造函数的另一种写法：

```js
class Person{
    sayHi(){
        console.log("Hi!")
    }
}
typeof Person	//function
Person === Person.prototype.constructor	//true
```

上面代码表明，类的数据类型就是函数，类本身就指向构造函数。使用的时候，也是直接对类使用`new`命令，跟构造函数的用法完全一致：

```js
let tom = new Person()
tom.sayHi()	//Hi
```

构造函数的`prototype`属性，在 ES6 的“类”上面继续存在。事实上，类的所有方法都定义在类的`prototype`属性上面。

```js
class Person{
    constructor(){}
    sayHi(){}
    //...
}
//等同于
function Person(){}
Person.prototype = {
    constructor(){}
    sayHi(){}
	//...
}
```

> 由于类的方法都定义在`prototype`对象上面，所以类的新方法可以添加在`prototype`对象上面。`Object.assign`方法可以很方便地一次向类添加多个方法。
>
> ```js
> class Person{
>     constructor(){
>         //...
>     }
> }
> Object.assign(Person.prototype,{
>     //在此添加新的属性和方法
> })
> ```

## 基本使用

### constructor方法

心态崩了，做了那么久的笔记怎么没保存上。