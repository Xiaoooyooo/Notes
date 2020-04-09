# ES6-Iterator

## Iterator（遍历器）的概念

它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

**Iterator 的作用有三个：**

+ 为各种数据结构，提供一个统一的、简便的访问接口；
+ 使得数据结构的成员能够按某种次序排列；
+ ES6 创造了一种新的遍历命令`for...of`循环，Iterator 接口主要供`for...of`使用。

**Iterator 的遍历过程：**

1. 创建一个指针对象，指向当前数据结构的起始位置。也就是说，**遍历器对象本质上，就是一个指针对象**。

2. 第一次调用指针对象的`next`方法，可以将指针指向数据结构的第一个成员。

3. 第二次调用指针对象的`next`方法，指针就指向数据结构的第二个成员。

4. 不断调用指针对象的`next`方法，直到它指向数据结构的结束位置。

每一次调用`next`方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含`value`和`done`两个属性的对象。其中，`value`属性是当前成员的值，`done`属性是一个布尔值，表示遍历是否结束。

```js
//模拟 next 方法的例子
function testIterator(arr){
    let currentIndex = 0
    return currentIndex == arr.length ? 
        { value:undefined,done:true } :
    	{ value:arr[currentIndex++],done:false }
}
let test = testItrator([1,2,3])
test.next()		//{value: 1, done: false}
test.next()		//{value: 2, done: false}
text.next()		//{value: 3, done: false}
test.next()		//{value: undefined, done: true}
```

## 默认 Iterator 接口

Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即`for...of`循环。当使用`for...of`循环遍历某种数据结构时，该循环会自动去寻找 Iterator 接口。一种数据结构只要部署了 Iterator 接口，我们就称这种数据结构是“可遍历的”（iterable）。

ES6 规定，默认的 Iterator 接口部署在数据结构的`Symbol.iterator`属性，或者说，一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”（iterable）。`Symbol.iterator`属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。

原生具有Iterator接口的数据结构有：

+ Array
+ Map
+ Set
+ String
+ TypedArray
+ 函数的arguments
+ Nodelist对象

```js
let arr = ['a','b','c','d']
let it = arr[Symbol.iterator]()
it.next()	//{value: "a", done: false}
it.next()	//{value: "b", done: false}
it.next()	//{value: "c", done: false}
it.next()	//{value: "d", done: false}
it.next()	//{value: undefined, done: true}
```

## 调用 Iterator 接口的场合

1. **解构赋值**

   对数组和 Set 结构进行解构赋值时，会默认调用`Symbol.iterator`方法。

   ```js
   let set = new Set().add('a').add('b').add('c')
   let [x,y] = set
   x	//a
   y	//b
   ```

2. **扩展运算符**

   扩展运算符（...）也会调用默认的 Iterator 接口。**可以将任何部署了 Iterator 接口的数据结构，转为数组**

   ```js
   let str = 'Hello'
   let [...arr] = str
   arr	//['H','e','l','l','o']
   
   let arr = ['c','d']
   arr = ['a',...arr,'d']
   arr	//['a','b','c','d']
   ```

3. **yield\***

   `yield*`后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。

4. **其他场合**

   由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用了遍历器接口。

## Iterator 接口与 Generator 函数

`Symbol.iterator`方法的最简单实现，还是使用 Generator 函数。

```js
let obj = {
    [Symbol.iterator]:function*(){
        yield 1
        yield 2
        yield 3
    }
}
[...obj]	//[1,2,3]
```

## 遍历器对象的 return()，throw()

遍历器对象除了具有`next`方法，还可以具有`return`方法和`throw`方法。如果你自己写遍历器对象生成函数，那么`next`方法是必须部署的，`return`方法和`throw`方法是否部署是可选的。

`return`方法的使用场合是，如果`for...of`循环提前退出（通常是因为出错，或者有`break`语句），就会调用`return`方法。如果一个对象在完成遍历前，需要清理或释放资源，就可以部署`return`方法。

**`return`方法必须返回一个对象**

```js
let obj = {
    foo:"foo",
    bar:"bar"
}
obj[Symbol.iterator] = function(){
    let values = Object.values(this)
    let current = 0
    return {
        next(){
			return current == values.length ? {
                value:undefined,
                done:true
            } : {
                value:values[current++],
                done:false
            }
        },
        return(){
			//提前退出时调用（出错或者break）
            console.log("提前退出")
            return {}
        }
    }
}
for(let value of obj){
    console.log(value)
    if(value === 'foo')
        break
}
//foo
//提前退出
```

---

## for...of 循环

ES6 借鉴 C++、Java、C# 和 Python 语言，引入了`for...of`循环，**作为遍历所有数据结构的统一的方法**。

一个数据结构只要部署了`Symbol.iterator`属性，就被视为具有 iterator 接口，就可以用`for...of`循环遍历它的成员。