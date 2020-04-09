# ES6-Promise

Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。

`Promise`对象有以下两个特点：

+ **对象的状态不受外界影响**。`Promise`对象代表一个异步操作，有三种状态：**`pending`（进行中）**、**`fulfilled`（已成功）**和**`rejected`（已失败）**。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
+ **一旦状态改变，就不会再变，任何时候都可以得到这个结果**。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。

`Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

## 基本用法

ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。

```js
let pro = new Promise(function(resolve,reject){
    let num = Math.random()
    if(num <= 0.5){
        resolve(num)
    }else{
        reject(num)
    }
})
```

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

```js
pro.then(function(message){
    //success
},function(err){
    //failure
})
```

## Promise.prototype.then()

Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象`Promise.prototype`上的。它的作用是**为 Promise 实例添加状态改变时的回调函数**。

接受两个参数，分别是两个回调函数：

+ `resolved`状态执行的回调函数

+ `rejected`状态执行的回调函数（可选）

返回一个新的`Promise`实例。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。

```js
new Promise(function(resolve,reject){
    let n = Math.random()
    n <= 0.5 ? resolve(n) : reject(n)
}).then(function(n){
    console.log(n)
    return 'Success'
},function(n){
    console.log(n)
    return 'Failure'
}).then(function(msg){
    console.log(msg)
},function(msg){
    console.log(msg)
})
```

## Promise.prototype.catch()

`Promise.prototype.catch`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

接受一个错误消息作为参数。

返回一个新的Promise对象。因此后面还可以接着调用`then`方法。

## Promise.prototype.finally()

`finally`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

接受一个回调函数作为参数，该回调不接受任何参数。

返回一个设置了 `finally` 回调函数的`Promise`对象。

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

## Promise.all()

`Promise.all`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

接受一个数组作为参数，数组内为Promise实例，如果不是，就会先调用`Promise.resolve`方法，将参数转为 Promise 实例，再进一步处理。

```js
let pro = Promise.all([p1,p2,p3])
```

上面例子中的`pro`的状态由`p1`、`p2`、`p3`决定，分成两种情况：

+ 只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

+ 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

**注意：**注意，如果作为参数的 Promise 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法。

## Promise.race()

基本与`Promise.all`相同，与`Promise.all`不同的是，只要有一个单独的实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。

```js
let pro = Promise.race([p1,p2,p3])
//p1,p2,p3中任意一个改变了状态，pro的状态就跟着改变
```

## Promise.resolve()

将指定对象转为Promise对象。

接受参数类型：

+ **参数是一个 Promise 实例**

  `Promise.resolve`将不做任何修改、原封不动地返回这个实例。

+ **参数是一个**`thenable`**对象**

  > `thenable`对象指的是具有`then`方法的对象，比如下面这个对象。
  >
  > ```js
  > let thenable = {
  >   then: function(resolve, reject) {
  >     resolve(42);
  >   }
  > };
  > ```

  `Promise.resolve`方法会将这个对象转为 Promise 对象，然后就立即执行`thenable`对象的`then`方法。

+ **参数不是具有**`then`**方法的对象，或根本就不是对象**

  如果参数是一个原始值，或者是一个不具有`then`方法的对象，则`Promise.resolve`方法返回一个新的 Promise 对象，状态为`resolved`。

  ```js
  let pro = Promise.resolve('foo')
  pro.then(msg => console.log(msg))	//foo
  ```

+ **不带有任何参数**

  直接返回一个`resolved`状态的 Promise 对象。所以，如果希望得到一个 Promise 对象，比较方便的方法就是直接调用`Promise.resolve()`方法。

  ```js
  const p = Promise.resolve();
  
  p.then(function () {
    // ...
  });
  ```

  **注意：**立即`resolve()`的 Promise 对象，是在本轮“事件循环”（event loop）的结束时执行，而不是在下一轮“事件循环”的开始时。

  ```js
  setTimeout(function () {
    console.log('three');
  }, 0);
  
  Promise.resolve().then(function () {
    console.log('two');
  });
  
  console.log('one');
  
  // one
  // two
  // three
  ```

## Promise.reject()

`Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected`。

```js
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```

**注意：**`Promise.reject()`方法的参数，**会原封不动地作为`reject`的理由**，变成后续方法的参数。这一点与`Promise.resolve`方法不一致。

```js
let thenable = {
    then:function(){
        reject("Reject")
    }
}
let pro = Promise.reject(thenable)
pro.catch(e => console.log(e === thenable))		//true
```

