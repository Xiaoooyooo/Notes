# JavaScript中的内存泄漏

## 什么是内存泄漏？

简单来说，内存泄漏就是不再使用的变量所占用的内存没有及时被释放，否则内存占用越来越高，轻则影响系统性能，重则导致进程崩溃。

在JavaScript中有两种变量，全局变量和在函数中产生的局部变量。局部变量的生命周期在函数执行过后就结束了，此时便可将它引用的内存释放（即垃圾回收），但全局变量生命周期会持续到浏览器关闭页面。所以当我们过多的使用全局变量的时候也会导致内存泄漏的问题。

## 垃圾回收机制

大多数语言都提供自动内存管理，这被称为"垃圾回收机制"（garbage collector）。

垃圾回收机制是如何直到哪些内存是不需要的呢？最常用的方法是通过“引用计数”， 即如果一个值的引用次数是`0`，就表示这个值不再用到了，因此可以将这块内存释放。

```js
const arr = [1,2,3,4,5]
console.log(123)
```

上面代码中`[1,2,3,4,5]`是一个值，被变量`arr`引用，因此它的引用次数是`1`。虽然后面的代码没有用到`arr`这个变量，但是它会一直占用一部分内存。

如果在最后增加一行代码`arr=null`，解除`arr`对`[1,2,3,4,5]`的引用，那么它此时的引用次数为`0`，占用的内存也会被垃圾回收机制释放了。

```js
const arr = [1,2,3,4,5]
console.log(123)
arr = null
```

但是，并不是说有了垃圾回收机制，我们就轻松了。

我们还是需要关注内存占用：那些很占空间的值，一旦不再用到，我们必须检查是否还存在对它们的引用。如果是的话，就必须手动解除引用。 

## JavaScript中可能造成内存泄漏的原因

+ JS

  + 意外的全局变量
  + 闭包使用不当
  + 模块形成的闭包内部变量使用完后没有置成null
  + 使用第三方库创建，没有调用正确的销毁函数

  ```js
  //一个因为闭包而引起的内存泄漏
  var theThing = null
  let replaceThing = function () {
    let originalThing = theThing
    let unused = function(){
      if (originalThing) {
        console.log("some message.")
      }
    }
    theThing = {
      str:new Array(10000).join('*'),
      someMethod() {
        console.log("Hello World")
      }
    }
  }
  setInterval(replaceThing, 1000)
  ```

+ DOM

  + 监听在window/body等事件没有解绑
  + 对DOM对象的引用
  + 在动态添加移除的DOM元素上绑定事件

## 参考资料

+ [JavaScript 内存泄漏教程](http://www.ruanyifeng.com/blog/2017/04/memory-leak.html)
+ [js造成内存泄漏的几种情况](https://www.cnblogs.com/xiaocuncheng/p/11101698.html)