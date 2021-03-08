---
title: ES6-Generator函数
date: 2019-10-07 21:10:19
tags:
	- JavaScript
	- ES6
categories:
	- JavaScript
---

# ES6-Generator函数

形式上，Generator 函数是一个普通函数，但是有两个特征。一是，`function`关键字与函数名之间有一个星号；二是，函数体内部使用`yield`表达式，定义不同的内部状态（`yield`在英语里的意思就是“产出”）。

```js
function* hello(){
    yield 'hello'
    yield 'world'
    return 'ending'
}
//定义一个Generator函数，该函数有三个状态：两个yield一个return（结束执行）
let h = hello()
```

Generator 函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。不同的是，调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是遍历器对象（Iterator Object）。

下一步，必须调用遍历器对象的`next`方法，使得指针移向下一个状态。也就是说，每次调用`next`方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个`yield`表达式（或`return`语句）为止。换言之，Generator 函数是分段执行的，`yield`表达式是暂停执行的标记，而`next`方法可以恢复执行。

```js
h.next()	//{value: "hello", done: false}
h.next()	//{value: "world", done: false}
h.next()	//{value: "ending", done: true}
h.next()	//{value: undefined, done: true}
```

## yield 表达式

由于 Generator 函数返回的遍历器对象，只有调用`next`方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。`yield`表达式就是暂停标志。

遍历器对象的`next`方法的运行逻辑如下：

1. 遇到`yield`表达式，就暂停执行后面的操作，并将紧跟在`yield`后面的那个表达式的值，作为返回的对象的`value`属性值。
2. 下一次调用`next`方法时，再继续往下执行，直到遇到下一个`yield`表达式。
3. 如果没有再遇到新的`yield`表达式，就一直运行到函数结束，直到`return`语句为止，并将`return`语句后面的表达式的值，作为返回的对象的`value`属性值。
4. 如果该函数没有`return`语句，则返回的对象的`value`属性值为`undefined`。

**注意：**`yield`表达式后面的表达式，只有当调用`next`方法、内部指针指向该语句时才会执行，因此等于为 JavaScript 提供了手动的“惰性求值”（Lazy Evaluation）的语法功能。

```js
function* fn(){
    console.log(0)
    yield 0
    console.log(1)
    yield 1
    console.log(2)
    return 2
}
let f = fn()	//初次调用并不会立即执行Generator函数里面的代码
f.next()
//0
//{value: 0, done: false}
f.next()
//1
//{value: 1, done: false}
f.next()
//2
//{value: 2, done: true}
f.next()
//{value: undefined, done: true}
```

## 与 Iterator 接口的关系

任意一个对象的`Symbol.iterator`方法，等于该对象的遍历器生成函数，调用该函数会返回该对象的一个遍历器对象。由于 Generator 函数就是遍历器生成函数，因此可以把 Generator 赋值给对象的`Symbol.iterator`属性，从而使得该对象具有 Iterator 接口。

```js
let obj = {}
obj[Symbol.iterator] = function*(){
    yield 1
    yield 2
    yield 3
    return 'end'
}
[...obj]	//[1,2,3]
```

## next 方法的参数

`yield`表达式本身没有返回值，或者说总是返回`undefined`。`next`方法可以带一个参数，该参数就会被当作上一个`yield`表达式的返回值。

```js
function* gen(){
    for(let i = 0;true;i++){
        let last = yield i	//last相当于调用next方法时传入的参数
        i = last == 1 ? -1 : i
    }
}
let g = gen()
g.next()	//{value: 0, done: false}
g.next()	//{value: 1, done: false}
g.next()	//{value: 2, done: false}
g.next(1)	//{value: 0, done: false}
```

这个功能有很重要的语法意义。Generator 函数从暂停状态到恢复运行，它的上下文状态（context）是不变的。通过`next`方法的参数，就有办法在 Generator 函数开始运行之后，继续向函数体内部注入值。也就是说，可以在 Generator 函数运行的不同阶段，从外部向内部注入不同的值，从而调整函数行为。

```js
function* gen(x){
    let y = yield (x + 1)
    let z = 2 * (yield (y / 3))
    return x + y + z
}
let g = gen(5)
g.next()	//{value: 6, done: false}
g.next()	//{value: NaN, done: false}
g.next()	//{value: NaN, done: true}

let g1 = gen(5)
g.next()	//{value: 6, done: false}
g.next(6)	//{value: 2, done: false}
g.next(6)	//{value: 23, done: true}
```

**注意：**由于`next`方法的参数表示上一个`yield`表达式的返回值，所以在第一次使用`next`方法时，传递参数是无效的。从语义上讲，第一个`next`方法用来启动遍历器对象，所以不用带有参数。

### 通过next方法向Generator函数内输入值

```js
function*gen(){
    console.log("Start")
    console.log(`${yield}`)
	console.log(`${yield}`)
	return 'ends'
}
let g = gen()
g.next()			//Start
g.next("Hello")		//Hello
g.next("World")		//World
```

## for...of 循环

`for...of`循环可以自动遍历 Generator 函数运行时生成的`Iterator`对象，且此时不再需要调用`next`方法。

```js
function*gen(){
    yield 1
    yield 2
    yield 3
    return 0
}
let g = gen()
for(let i of g){
    console.log(i)
}
//1
//2
//3
```

需要注意的是，一旦`next`方法的返回对象的`done`属性为`true`，`for...of`循环就会中止，且不包含该返回对象，所以return后面的值，不包括在`for...of`循环之中。

## Generator.prototype.throw()

Generator 函数返回的遍历器对象，都有一个`throw`方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

```js
function*gen(){
    try{
        yield 1
        return 0
    }catch(e){
        console.log("inside",e)
    }
}
let g = gen()
g.next()
try{
    g.throw('message1')
    g.throw('message2')
}catch(e){
    console.log("outside",e)
}
//inside message1
//outside message2
```

`throw`方法抛出的错误要被内部捕获，前提是必须至少执行过一次`next`方法。

第一个错误被 Generator 函数体内的`catch`语句捕获。`i`第二次抛出错误，由于 Generator 函数内部的`catch`语句已经执行过了，不会再捕捉到这个错误了，所以这个错误就被抛出了 Generator 函数体，被函数体外的`catch`语句捕获。

## Generator.prototype.return()

Generator 函数返回的遍历器对象，还有一个`return`方法，可以返回给定的值，并且终结遍历 Generator 函数。

如果`return`方法调用时，不提供参数，则返回值的`value`属性为`undefined`。

```js
function*gen(){
    yield 1
    yield 2
    yield 3
    return 0
}
let g = gen()
g.next()		//{value: 1, done: false}
g.return("foo")	//{value: "foo", done: true}
g.next()		//{value: undefined, done: true}
```

如果 Generator 函数内部有`try...finally`代码块，**且正在执行`try`代码块**，那么`return`方法会导致立刻进入`finally`代码块，**执行完以后，整个函数才会结束**。

```js
function*gen(){
    yield 1
    yield 2
    try{
        yield 3
        yield 4
    }finally{
        yield 5
        yield 6
    }
    yield 7
    yield 8
}
let g = gen()
g.next()	//{value: 1, done: false}
g.next()	//{value: 2, done: false}
g.next()	//{value: 3, done: false}
g.return("foo")	//{value: 5, done: false}
g.next()	//{value: 6, done: false}
g.next()	//{value: "foo", done: true}
```

## yield\* 表达式

如果在 Generator 函数内部，调用另一个 Generator 函数。需要在前者的函数体内部，自己手动完成遍历。

```js
function*foo(){
    yield 1
    yield 2
    yield 3
    return 0
}
function*bar(){
    yield "x"
    for(let i of foo()){
        console.log(i)
    }
    yield "y"
    return 'end'
}
for(let b of bar()){
    console.log(b)
}
/**
 * x
 * 1
 * 2
 * 3
 * y
 */
```

ES6 提供了`yield*`表达式，作为解决办法，用来在一个 Generator 函数里面执行另一个 Generator 函数。

```js
function*foo(){
    yield 1
    yield 2
    yield 3
}
function*bar(){
    yield 'a'
    yield* foo()
    yield 'b'
}
for(let i of bar()){
    console.log(i)
}
/**
 * x
 * 1
 * 2
 * 3
 * y
 */
```

实际上，任何数据结构只要有 Iterator 接口，就可以被`yield*`遍历。

如果被代理的 Generator 函数有`return`语句，那么就可以向代理它的 Generator 函数返回数据。

```js
function*foo(){
    yield 1
    yield 2
    return 'end'
}
function*bar(){
    yield 'a'
    let res = yield* foo()
    yield res
    yield 'b'
}
for(let i of bar()){
    console.log(i)
}
/**
 * a
 * 1
 * 2
 * end
 * b
 */
```

## 作为对象属性的 Generator 函数

如果一个对象的属性是 Generator 函数，可以简写成下面的形式。

```js
let obj = {
    *genProperty(){
        //...
    },
    //等价于
    genProperty:function*(){
        //...
    }
}
//genProperty属性前面有一个星号，表示这个属性是一个 Generator 函数。
```

## Generator 函数的this

Generator 函数总是返回一个遍历器，ES6 规定这个遍历器是 Generator 函数的实例，也继承了 Generator 函数的`prototype`对象上的方法。

```js
function* gen(){}
gen.prototype.foo = "foo"
let g = gen()
g instanceof gen	//true
g.foo	//foo
```

上面代码表明，Generator 函数`gen`返回的遍历器`g`，是`gen`的实例，而且继承了`gen.prototype`。但是，如果把`gen`当作普通的构造函数，并不会生效，因为`gen`返回的总是遍历器对象，而不是`this`对象。

```js
function* g() {
  this.a = 11;
}

let obj = g();
obj.next();
obj.a // undefined
window.a	//11
```

Generator 函数也不能跟`new`命令一起用，会报错。

如果需要是Generator函数中的this绑定到特定的‘实例对象’，可以这样做：

```js
function*gen(){
    yield this.a = 1
    yield this.b = 2
    yield this.c = 3
}
let obj = {}	//新建一个空对象
let g = gen.apply(obj)
for(let i of g){
    console.log(i)
}
//1 2 3
obj	//{a: 1, b: 2, c: 3}
```

## 含义

### Generator 与状态机

Generator 是实现状态机的最佳结构。

```js
//ES5实现
var ticking = true
var clock = function(){
    if(ticking)
        console.log("Tick!")
    else
        console.log("Tock!")
    ticking = !ticking
}

//Generator实现
let clock = function*(){
    while(true){
        console.log("Tick!")
        yield
        console.log("Tock!")
        yield
    }
}
```

Generator 实现与 ES5 实现对比，可以看到少了用来保存状态的外部变量`ticking`，这样就更简洁，更安全（状态不会被非法篡改）、更符合函数式编程的思想，在写法上也更优雅。Generator 之所以可以不用外部变量保存状态，是因为它本身就包含了一个状态信息，即目前是否处于暂停态。

### Generator 与协程

协程（coroutine）是一种程序运行的方式，可以理解成“协作的线程”或“协作的函数”。协程既可以用单线程实现，也可以用多线程实现。前者是一种特殊的子例程，后者是一种特殊的线程。

#### 协程与子例程的差异

传统的“子例程”（subroutine）采用堆栈式“后进先出”的执行方式，只有当调用的子函数完全执行完毕，才会结束执行父函数。协程与其不同，多个线程（单线程情况下，即多个函数）可以并行执行，但是只有一个线程（或函数）处于正在运行的状态，其他线程（或函数）都处于暂停态（suspended），线程（或函数）之间可以交换执行权。也就是说，一个线程（或函数）执行到一半，可以暂停执行，将执行权交给另一个线程（或函数），等到稍后收回执行权的时候，再恢复执行。这种可以并行执行、交换执行权的线程（或函数），就称为协程。

**从实现上看，在内存中，子例程只使用一个栈（stack），而协程是同时存在多个栈，但只有一个栈是在运行状态，也就是说，协程是以多占用内存为代价，实现多任务的并行**。

#### 协程与普通线程的差异

不难看出，协程适合用于多任务运行的环境。在这个意义上，它与普通的线程很相似，都有自己的执行上下文、可以分享全局变量。它们的不同之处在于，同一时间可以有多个线程处于运行状态，但是运行的协程只能有一个，其他协程都处于暂停状态。此外，普通的线程是抢先式的，到底哪个线程优先得到资源，必须由运行环境决定，但是协程是合作式的，执行权由协程自己分配。

由于 JavaScript 是单线程语言，只能保持一个调用栈。引入协程以后，每个任务可以保持自己的调用栈。这样做的最大好处，就是抛出错误的时候，可以找到原始的调用栈。不至于像异步操作的回调函数那样，一旦出错，原始的调用栈早就结束。

Generator 函数是 ES6 对协程的实现，但属于不完全实现。Generator 函数被称为“半协程”（semi-coroutine），意思是**只有 Generator 函数的调用者，才能将程序的执行权还给 Generator 函数**。如果是完全执行的协程，任何函数都可以让暂停的协程继续执行。

**如果将 Generator 函数当作协程，完全可以将多个需要互相协作的任务写成 Generator 函数，它们之间使用`yield`表达式交换控制权**。

### Generator 与上下文

JavaScript 代码运行时，会产生一个全局的上下文环境（context，又称运行环境），包含了当前所有的变量和对象。然后，执行函数（或块级代码）的时候，又会在当前上下文环境的上层，产生一个函数运行的上下文，变成当前（active）的上下文，由此形成一个上下文环境的堆栈（context stack）。

这个堆栈是“后进先出”的数据结构，最后产生的上下文环境首先执行完成，退出堆栈，然后再执行完成它下层的上下文，直至所有代码执行完成，堆栈清空。

Generator 函数不是这样，它执行产生的上下文环境，**一旦遇到`yield`命令，就会暂时退出堆栈，但是并不消失，里面的所有变量和对象会冻结在当前状态**。等到对它执行`next`命令时，这个上下文环境又会重新加入调用栈，冻结的变量和对象恢复执行。

---

## 应用

Generator 可以暂停函数执行，返回任意表达式的值。这种特点使得 Generator 有多种应用场景。

1. **异步操作的同步化表达**

   Generator 函数的暂停执行的效果，意味着可以把异步操作写在`yield`表达式里面，等到调用`next`方法时再往后执行。

   ```js
   function*gen(){
       doSomethig()
       yield
       doSomethigElse()
   }
   let g = gen()
   g.next()	//第一步操作
   //一系列操作完成过后
   g.next()	//第二步操作
   ```

2. **控制流管理**

   如果有一个多步操作非常耗时，采用回调函数，可能会写成下面这样：

   ```js
   step1(function (value1) {
     step2(value1, function(value2) {
       step3(value2, function(value3) {
         step4(value3, function(value4) {
           // Do something with value4
         });
       });
     });
   });
   ```

   采用 Promise 改写上面的代码:

   ```js
   Promise.resolve(step1)
     .then(step2)
     .then(step3)
     .then(step4)
     .then(function (value4) {
       // Do something with value4
     }, function (error) {
       // Handle any error from step1 through step4
     })
     .done();
   ```

   上面代码已经把回调函数，改成了直线执行的形式，但是加入了大量 Promise 的语法。Generator 函数可以进一步改善代码运行流程：

   ```js
   function* longRunningTask(value1) {
     try {
       var value2 = yield step1(value1);
       var value3 = yield step2(value2);
       var value4 = yield step3(value3);
       var value5 = yield step4(value4);
       // Do something with value4
     } catch (e) {
       // Handle any error from step1 through step4
     }
   }
   ```

   **注意：**只适用于同步任务

3. **部署 Iterator 接口**

   利用 Generator 函数，可以在任意对象上部署 Iterator 接口。

4. **作为数据结构**

   Generator 可以看作是数据结构，更确切地说，可以看作是一个数组结构，因为 Generator 函数可以返回一系列的值，这意味着它可以对任意表达式，提供类似数组的接口。

   ```js
   function* doStuff() {
     yield fs.readFile.bind(null, 'hello.txt');
     yield fs.readFile.bind(null, 'world.txt');
     yield fs.readFile.bind(null, 'and-such.txt');
   }
   for (task of doStuff()) {
     // task是一个函数，可以像回调函数那样使用它
   }
   ```

## Generator异步应用

ES6 诞生以前，异步编程的方法，大概有下面四种：

1. 回调函数

2. 事件监听

3. 发布/订阅

4. Promise 对象

Generator 函数将 JavaScript 异步编程带入了一个全新的阶段。

### Generator 函数

#### 协程

传统的编程语言，早有异步编程的解决方案（其实是多任务的解决方案）。其中有一种叫做"协程"（coroutine），意思是多个线程互相协作，完成异步任务。

协程有点像函数，又有点像线程。它的运行流程大致如下：

- 第一步，协程`A`开始执行。
- 第二步，协程`A`执行到一半，进入暂停，执行权转移到协程`B`。
- 第三步，（一段时间后）协程`B`交还执行权。
- 第四步，协程`A`恢复执行。

```js
//读取文件的协程写法
function* asyncJob() {
  // ...其他代码
  var f = yield readFile(fileA);
  // ...其他代码
}
```

上面代码的函数`asyncJob`是一个协程，它的奥妙就在其中的`yield`命令。它表示执行到此处，执行权将交给其他协程。也就是说，`yield`命令是异步两个阶段的分界线。协程遇到`yield`命令就暂停，等到执行权返回，再从暂停的地方继续往后执行。它的最大优点，就是代码的写法非常像同步操作，如果去除`yield`命令，简直一模一样。

#### 异步任务的封装

如何使用 Generator 函数，执行一个真实的异步任务：

```js
var fetch = require('node-fetch');

function* gen(){
  var url = 'https://api.github.com/users/github';
  var result = yield fetch(url);
  console.log(result.bio);
}

//执行这段代码
var g = gen();
var result = g.next();

result.value.then(function(data){
  return data.json();
}).then(function(data){
  g.next(data);
});
```

可以看到，虽然 Generator 函数将异步操作表示得很简洁，但是流程管理却不方便（即何时执行第一阶段、何时执行第二阶段）。

### Thunk 函数

Thunk 函数是自动执行 Generator 函数的一种方法。

#### JavaScript 语言的 Thunk 函数

JavaScript 语言是传值调用，它的 Thunk 函数含义有所不同。在 JavaScript 语言中，Thunk 函数替换的不是表达式，而是多参数函数，将其替换成一个只接受回调函数作为参数的单参数函数。

```js
// 正常版本的readFile（多参数版本）
fs.readFile(fileName, callback);

// Thunk版本的readFile（单参数版本）
var Thunk = function (fileName) {
  return function (callback) {
    return fs.readFile(fileName, callback);
  };
};

var readFileThunk = Thunk(fileName);
readFileThunk(callback);
```

上面代码中，`fs`模块的`readFile`方法是一个多参数函数，两个参数分别为文件名和回调函数。经过转换器处理，它变成了一个单参数函数，只接受回调函数作为参数。**这个单参数版本，就叫做 Thunk 函数**。任何函数，只要参数有回调函数，就能写成 Thunk 函数的形式。

#### Generator 函数的流程管理

你可能会问， Thunk 函数有什么用？回答是以前确实没什么用，但是 ES6 有了 Generator 函数，Thunk 函数现在可以用于 Generator 函数的自动流程管理。

```js
function* gen() {
  // ...
}

var g = gen();
var res = g.next();

while(!res.done){
  console.log(res.value);
  res = g.next();
}
```

上面代码中，Generator 函数`gen`会自动执行完所有步骤。但是，**这不适合异步操作**。如果必须保证前一步执行完，才能执行后一步，上面的自动执行就不可行。这时，Thunk 函数就能派上用处。以读取文件为例。下面的 Generator 函数封装了两个异步操作：

```js
var fs = require('fs');
var thunkify = require('thunkify');
var readFileThunk = thunkify(fs.readFile);

var gen = function* (){
  var r1 = yield readFileThunk('/etc/fstab');
  console.log(r1.toString());
  var r2 = yield readFileThunk('/etc/shells');
  console.log(r2.toString());
};

var g = gen();
var r1 = g.next();	//r1.value = readFileThunk('/etc/fstab')
r1.value(function (err, data) {
  if (err) throw err;
  var r2 = g.next(data);
  r2.value(function (err, data) {
    if (err) throw err;
    g.next(data);
  });
});
```

#### Thunk 函数的自动流程管理

Thunk 函数真正的威力，在于可以自动执行 Generator 函数。下面就是一个基于 Thunk 函数的 Generator 执行器：

```js
function run(fn) {
  var gen = fn();

  function next(err, data) {
    var result = gen.next(data);
    if (result.done) return;
    result.value(next);
  }

  next();
}

function* g() {
    //内部每一个yield都必须返回一个Thunk函数
  	var f1 = yield readFileThunk('fileA');
  	var f2 = yield readFileThunk('fileB');
  	// ...
  	var fn = yield readFileThunk('fileN');
}

run(g);
```

上面代码中，函数`g`封装了`n`个异步的读取文件操作，只要执行`run`函数，这些操作就会自动完成。这样一来，异步操作不仅可以写得像同步操作，而且一行代码就可以执行。

Thunk 函数并不是 Generator 函数自动执行的唯一方案。因为自动执行的关键是，必须有一种机制，自动控制 Generator 函数的流程，接收和交还程序的执行权。回调函数可以做到这一点，Promise 对象也可以做到这一点。

## co 模块

co 模块是著名程序员 TJ Holowaychuk 于 2013 年 6 月发布的一个小工具，用于 Generator 函数的自动执行。

```js
var gen = function* () {
  var f1 = yield readFile('/etc/fstab');
  var f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};

var co = require('co');
//Generator 函数只要传入co函数，就会自动执行
co(gen);
//co函数返回一个Promise对象，因此可以用then方法添加回调函数。
co(gen).then(function (){
  console.log('Generator 函数执行完成');
});
```

