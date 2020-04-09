# ES6-Reflect

`Reflect`对象与`Proxy`对象一样，也是 ES6 为了操作对象而提供的新 API。`Reflect`对象的设计目的有这样几个：

+ **将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`），放到`Reflect`对象上**。现阶段，某些方法同时在`Object`和`Reflect`对象上部署，未来的新方法将只部署在`Reflect`对象上。也就是说，从`Reflect`对象上可以拿到语言内部的方法。

+ **修改某些`Object`方法的返回结果，让其变得更合理**。比如，`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`。

  ```js
  // 老写法
  try {
    Object.defineProperty(target, property, attributes);
    // success
  } catch (e) {
    // failure
  }
  
  // 新写法
  if (Reflect.defineProperty(target, property, attributes)) {
    // success
  } else {
    // failure
  }
  ```

+ **让`Object`操作都变成函数行为**。某些`Object`操作是命令式，比如`name in obj`和`delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为。

  ```js
  // 老写法
  'assign' in Object // true
  // 新写法
  Reflect.has(Object, 'assign') // true
  ```

+ **`Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法**。这就让`Proxy`对象可以方便地调用对应的`Reflect`方法，完成默认行为，作为修改行为的基础。

  ```js
  //Proxy方法拦截target对象的属性赋值行为。它采用Reflect.set方法将值赋值给对象的属性，确保完成原有的行为，然后再部署额外的功能。
  Proxy(target, {
    set: function(target, name, value, receiver) {
      var success = Reflect.set(target, name, value, receiver);
      if (success) {
        console.log('property ' + name + ' on ' + target + ' set to ' + value);
      }
      return success;
    }
  });
  ```

有了`Reflect`对象以后，很多操作会更易读。

```js
// 老写法
Function.prototype.apply.call(Math.floor, undefined, [1.75]) // 1

// 新写法
Reflect.apply(Math.floor, undefined, [1.75]) // 1
```

## Reflect的静态方法

`Reflect`对象一共有 13 个静态方法，大部分与`Object`对象的同名方法的作用都是相同的，而且它与`Proxy`对象的方法是一一对应的：

+ **Reflect.apply(target, thisArg, args)**
+ **Reflect.construct(target, args)**
+ **Reflect.get(target, name, receiver)**
+ **Reflect.set(target, name, value, receiver)**
+ **Reflect.defineProperty(target, name, desc)**
+ **Reflect.deleteProperty(target, name)**
+ **Reflect.has(target, name)**
+ **Reflect.ownKeys(target)**
+ **Reflect.isExtensible(target)**
+ **Reflect.preventExtensions(target)**
+ **Reflect.getOwnPropertyDescriptor(target, name)**
+ **Reflect.getPrototypeOf(target)**
+ **Reflect.setPrototypeOf(target, prototype)**

### 1. Reflect.get(target, name, receiver)

`Reflect.get`方法查找并返回`target`对象的`name`属性，如果没有该属性，则返回`undefined`。

接受三个参数：

+ 需要取值的目标对象
+ 需要取值的键名
+ 如果`target`对象中指定了`getter`，则指定`getter`中的`this`指向。

返回取到的属性值。

```js
let obj = {
    foo:"foo",
    bar:'bar',
    get baz(){
        return 'baz'
    }
}
Reflect.get(obj,'foo')	//foo
Reflect.get(obj,'bar')	//bar
Reflect.get(obj,'baz')	//baz
```

如果`name`属性部署了读取函数（getter），则读取函数的`this`绑定`receiver`。

```js
let obj = {
    foo:"foo",
    bar:"bar",
    get baz(){
        return this.foo + this.bar
    }
}
let receiverObj = {
    foo: 1,
    bar: 2
}
Reflect.get(obj,'baz',receiverObj)	//3
```

### 2. Reflect.set(target, name, value, receiver)

`Reflect.set`方法设置`target`对象的`name`属性等于`value`。

接受四个参数：

+ 目标对象
+ 属性名
+ 需要设置的属性值
+ 如果遇到`setter`，则指定`setter`中的`this`指向

返回一个布尔值，表示是否设置成功。

```js
let obj = {
    set id(val){
        this._id = val
    }
}
let receiverObj = {}
Reflect.set(obj,'foo','foo')
Reflect.set(obj,'id','001')
obj		//{foo: "foo", _id: "001"}

Reflect.set(obj,'id','001',receiverObj)
receiver	//{_id: "001"}
```

### 3. Reflect.has(obj, name)

`Reflect.has`方法对应`name in obj`里面的`in`运算符。

接受两个参数：

+ 目标对象
+ 需要检测的属性名

返回一个布尔值，表示目标对象内是否有该属性。

```js
let obj = {
    foo:"foo"
}
Reflect.has(obj,'foo')	//true
Reflect.has(obj,'bar')	//false
```

### 4. Reflect.deleteProperty(obj, name)

`Reflect.deleteProperty`方法等同于`delete obj[name]`，用于删除对象的属性。

接受两个参数：

+ 目标对象
+ 需要删除的属性名

返回一个布尔值，`true`表示删除成功或被删除属性不存在；`false`表示删除失败，目标属性依然存在。

```js
let obj = {
    foo:"foo"
}
Reflect.deleteProperty(obj,"foo")	//true
Reflect.deleteProperty(obj,'bar')	//true
```

### 5. Reflect.construct(target, args)

`Reflect.construct`方法等同于`new target(...args)`，这提供了一种不使用`new`，来调用构造函数的方法。

接受三个参数：

+ 目标构造函数（`target`）
+ 类数组，目标构造函数调用时的参数（`argumentList`）
+ 新目标构造函数（`newTarget`）

返回一个以`target`（如果`newTarget`存在，则为`newTarget`）函数为构造函数，`argumentList`为其初始化参数的对象实例。

```js
let F = function(name,age){
        this.name = name
        this.age = age
}
let S = function(name,age){
    this.name = name
    this.age = age
}
let f = Reflect.construct(F,["tom",25])
/**
 * 上面的代码等同于下面
 * let f = Reflect.construct(F,{
 *       0:"Tom",
 *       1:25,
 *       length:2
 * })
 */
let ff = Reflect.construct(F,["tom",25],S)
f	//F {name: "tom", age: 25}
ff	//S {name: "tom", age: 25}
```

### 6. Reflect.getPrototypeOf(obj)

`Reflect.getPrototypeOf`方法用于读取对象的`__proto__`属性，对应`Object.getPrototypeOf(obj)`。

接受一个目标对象作为参数。

返回给定对象的原型。如果没有继承的属性，则返回 `null`

```js
let obj = {
    foo:"foo"
}
Reflect.getPrototypeOf(obj === obj.__proto__)	//true
```

`Reflect.getPrototypeOf`和`Object.getPrototypeOf`的一个区别是，如果参数不是对象，`Object.getPrototypeOf`会将这个参数转为对象，然后再运行，而`Reflect.getPrototypeOf`会报错。

### 7. Reflect.setPrototypeOf(obj, newProto)

`Reflect.setPrototypeOf`方法用于设置目标对象的原型（prototype），对应`Object.setPrototypeOf(obj, newProto)`方法。它返回一个布尔值，表示是否设置成功。

接受两个参数：

+ 目标对象
+ 原型对象

返回一个布尔值，表示设置是否成功。

```js
let obj = {}
Reflect.setPrototypeOf(obj,{foo:"foo"})
obj.__proto__	//{foo: "foo"}
```

### 8. Reflect.apply(func, thisArg, args)

`Reflect.apply`方法等同于`Function.prototype.apply.call(func, thisArg, args)`，用于绑定`this`对象后执行给定函数。

接受三个参数：

+ 目标函数
+ target函数调用时绑定的this对象
+ target函数执行时传入的实参列表（类数组对象）

返回值是调用完带着指定参数和 `this` 值的给定的函数后返回的结果。

```js
let fn = function(){
    console.log(this)
    return [...arguments]
}
let obj = {
    foo:"foo"
}
Reflect.apply(fn,obj,[1,2,3])
//{foo: "foo"}
//[1,2,3]
```

### 9. Reflect.defineProperty(target, propertyKey, attributes)

`Reflect.defineProperty`方法基本等同于`Object.defineProperty`，用来为对象定义属性。*未来，后者会被逐渐废除，请从现在开始就使用`Reflect.defineProperty`代替它。*

接受三个参数：

+ 目标对象
+ 目标属性名
+ 配置对象

返回一个布尔值，表示是否定义成功。

```js
let obj = {
    foo:"foo"
}
Reflect.defineProperty(obj,'foo',{
    value:"bar",
    writable:false,
    enumerable:false,
    configurable:false
})
Reflect.getOwnPropertyDescriptor(obj,'foo')
//{value: "bar", writable: false, enumerable: true, configurable: false}
```

### 10. Reflect.getOwnPropertyDescriptor(target, propertyKey)

`Reflect.getOwnPropertyDescriptor`基本等同于`Object.getOwnPropertyDescriptor`，用于得到指定属性的描述对象，将来会替代掉后者。

接受两个参数：

+ 目标对象
+ 目标属性名

如果属性存在于给定的目标对象中，则返回属性描述符；否则，返回 `undefined`。

```js
let obj = {
    foo:"foo"
}
Reflect.getOwnPropertyDescriptor(obj,"foo")
//{value: "foo", writable: true, enumerable: true, configurable: true}
```

### 11. Reflect.isExtensible (target)

`Reflect.isExtensible`方法对应`Object.isExtensible`，返回一个布尔值，表示当前对象是否可扩展。

接受一个目标对象作为参数。

返回一个布尔值，表示是否可扩展。

```js
let obj = {}
Reflect.isExtensible(obj)	//true
Reflect.preventExtensions(obj)
Reflect.isExtensible(obj)	//false
```

### 12. Reflect.preventExtensions(target)

`Reflect.preventExtensions`对应`Object.preventExtensions`方法，用于让一个对象变为不可扩展。它返回一个布尔值，表示是否操作成功。

接受一个目标对象作为参数。

返回一个布尔值，表示操作是否成功。

```js
let obj = {}
Reflect.isExtensible(obj)	//true
Reflect.preventExtensions(obj)	//true
Reflect.isExtensible(obj)	//false
```

### 13. Reflect.ownKeys (target)

`Reflect.ownKeys`方法用于返回对象的所有属性，基本等同于`Object.getOwnPropertyNames`与`Object.getOwnPropertySymbols`之和。

接受一个目标对象作为参数。

返回由目标对象键名组成的数组（包括`Symbol`键名和不可遍历属性）。

```js
let obj = {
    foo:"foo",
    bar:"bar",
    [Symbol("one")]:"one",
    [Symbol("two")]:"two"
}
Reflect.defineProperty(obj,"foo",{
        enumerable:false
})
Reflect.ownKeys(obj)	//["foo", "bar", Symbol(one), Symbol(two)]
```

