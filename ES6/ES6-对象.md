# ES6-对象

## 属性名表达式

```js
//设置对象属性时
obj.name = 'xxx'
obj['na'+'me'] = 'xxx'


let a = 'age'
let obj1 = {
    ['name']:'xxx',
    [a]:20,
    ['say' + 'hi'](){
        alert("Hi")
    }
}
```

## 对象属性的可枚举型和遍历

### 可枚举型

对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为。`Object.getOwnPropertyDescriptor`方法可以获取该属性的描述对象。

```js
let obj = {
    foo:"bar"
}
Object.getOwnPropertyDescriptor(obj,'foo')
//{
//    value: 'bar',			//属性值
//    writable: true,		//是否可重写
//    enumerable: true,		//是否可枚举
//    configurable: true	//是否可修改以上三个属性
//}
```

### 属性的遍历

ES6 一共有 5 种方法可以遍历对象的属性。

```js
let obj = {
    foo:'foo',
    bar:'bar',
    some:'some'
}
Object.defineProperty(obj,'some',{
    enumerable: false
})
```

1. **for...in**

   `for...in`循环遍历对象**自身的和继承的**可枚举属性（不含 Symbol 属性）。

   ```js
   for(let key in obj){
       console.log(key,obj[key])
   }
   //foo foo
   //bar bar
   ```

2. **Object.keys(obj)**

   `Object.keys`返回一个数组，包括对象**自身的（不含继承的）**所有可枚举属性（不含 Symbol 属性）的键名。

   ```js
   Object.keys(obj)
   //['foo','bar']
   ```

3. **Object.getOwnPropertyNames(obj)**

   返回一个数组，包含对象自身的所有属性（不含 Symbol 属性，但是**包括不可枚举属性**）的键名。

   ```js
   Object.getOwnPropertyNames(obj)
   //['foo','bar','some']
   ```

4. **Object.getOwnPropertySymbols(obj)**

   返回一个数组，包含对象自身的所有 Symbol 属性的键名。

   ```js
   Object.getOwnPropertySymbols(obj)
   //略
   ```

5. **Reflect.ownKeys(obj)**

   返回一个数组，包含对象自身的所有键名，不管键名是 Symbol 或字符串，也不管是否可枚举。
   
   ```js
   Reflect.ownKeys(obj)
   //['foo','bar','some']
   ```

## super关键字

`this`关键字总是指向函数所在的当前对象，ES6 又新增了另一个类似的关键字`super`，**指向当前对象的原型对象**。

`super`关键字表示原型对象时，只能用在对象的方法之中，用在其他地方都会报错。

```js
const proto = {
  foo: 'hello'
};

const obj = {
  foo: 'world',
  find() {
      console.log(super.foo)
      console.log(this.foo)
  }
};

Object.setPrototypeOf(obj, proto);
obj.find() // "hello" "world"
```

## 对象的扩展运算符（`...`）

用于取出参数对象的所有可遍历属性，此时等号赋值的右边必须是一个对象。

```js
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }
```

## 对象新增方法

### Object.is()

比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。不同之处只有两个：一是`+0`不等于`-0`，二是`NaN`等于自身。

```js
+0 === -0 //true
NaN === NaN //false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

### Object.assign()

用于对象的合并，将源对象（source）的**所有可枚举属性**，复制到目标对象（target）。

`Object.assign`的第一个参数是目标对象，后面的参数都是源对象

```js
let target = {
    a:'a',
    b:'b',
    c:'cc'
}
let source = {
    c:'c',
    d:'d'
}
Object.assign(target,source)
//target = {a: "a", b: "b", c: "c", d: "d"}
//source = {c: "c", d: "d"}
```

### Object.getOwnPropertyDescriptors()

> **Object.getOwnPropertyDescriptor()**

返回指定对象所有自身属性（非继承属性）的描述对象。

```js
let obj = {
    foo:"foo",
    get bar(){
            return "bar"
        }
}
Object.getOwnPropertyDescriptors(obj)
// { foo:
//    { value: 123,
//      writable: true,
//      enumerable: true,
//      configurable: true },
//   bar:
//    { get: [Function: get bar],
//      set: undefined,
//      enumerable: true,
//      configurable: true } }
```

### \__proto__属性，Object.setPrototypeOf()，Object.getPrototypeOf()

+ **\__proto__属性**

略

+ **Object.setPrototypeOf()**

用来设置一个对象的`prototype`对象，返回参数对象本身。它是 ES6 正式推荐的设置原型对象的方法。

```js
//用法
Object.setPrototypeOf(obj, prototype)
```

+ **Object.getPrototypeOf()**

该方法与`Object.setPrototypeOf`方法配套，用于读取一个对象的原型对象。

```js
//用法
Object.getPrototypeOf(obj)
```

### Object.keys()，Object.values()，Object.entries()

+ **Object.keys()**

  ES5 引入了`Object.keys`方法，返回一个数组，成员是参数对象自身的（**不含继承的**）所有**可遍历（enumerable）属性的**键名。

+ **Object.values()**

  返回一个数组，成员是参数对象自身的（**不含继承的**）所有**可遍历（enumerable）属性的**键值会过滤忽略属性名为 Symbol 值的属性

+ **Object.entries()**

  返回一个数组，成员是参数对象自身的（**不含继承的**）所有**可遍历（enumerable）属性的**键值对数组

  **都会忽略属性名为 Symbol 值的属性**

### Object.fromEntries()

`Object.entries()`的逆操作，用于将一个键值对数组转为对象。该方法的主要目的，是将键值对的数据结构还原为对象，因此特别适合将 Map 结构转为对象。

```js
Object.fromEntries([
  ['foo', 'bar'],
  ['baz', 42]
])
// { foo: "bar", baz: 42 }
```

该方法的一个用处是配合`URLSearchParams`对象，将查询字符串转为对象。

```js
Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
// { foo: "bar", baz: "qux" }
```

