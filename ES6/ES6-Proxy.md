# ES6-Proxy

Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例。其中，`new Proxy()`表示生成一个`Proxy`实例，`target`参数表示所要拦截的目标对象，`handler`参数也是一个对象，用来定制拦截行为。

```js
var proxy = new Proxy(target, handler);
```

`handler`中可以设置的拦截操作有13种：

1. **get(target, propKey, receiver)**：拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`。
2. **set(target, propKey, value, receiver)**：拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。
3. **has(target, propKey)**：拦截`propKey in proxy`的操作，返回一个布尔值。
4. **deleteProperty(target, propKey)**：拦截`delete proxy[propKey]`的操作，返回一个布尔值。
5. **ownKeys(target)**：拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`、`for...in`循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而`Object.keys()`的返回结果仅包括目标对象自身的可遍历属性。
6. **getOwnPropertyDescriptor(target, propKey)**：拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。
7. **defineProperty(target, propKey, propDesc)**：拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。
8. **preventExtensions(target)**：拦截`Object.preventExtensions(proxy)`，返回一个布尔值。
9. **getPrototypeOf(target)**：拦截`Object.getPrototypeOf(proxy)`，返回一个对象。
10. **isExtensible(target)**：拦截`Object.isExtensible(proxy)`，返回一个布尔值。
11. **setPrototypeOf(target, proto)**：拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
12. **apply(target, object, args)**：拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。
13. **construct(target, args)**：拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`。

## 拦截方法详细介绍

### 1. get()

用于拦截某个属性的读取操作，可以接受三个参数，依次为目标对象、属性名和 proxy 实例本身（可选）。

可以返回任何值。

```js
let person = {
    name:'Tom'
}
let proxy = new Proxy(person,{
    get:function(target,propertyname,proxy){
        if(propertyname in target) return target[propertyname]
        else throw new ReferenceError(`Property ${propertyname} doesn't exist.`)
    }
})
proxy.name	//Tom
proxy.age	//Proxy.js:15 Uncaught ReferenceError: Property age doesn't exits.
```

**注意：**如果一个属性**不可配置（configurable）且不可写（writable）**，则 Proxy 不能修改该属性，否则通过 Proxy 对象访问该属性会报错。

```js
const target = Object.defineProperties({}, {
  foo: {
    value: 123,
    writable: false,
    configurable: false
  }
});

const handler = {
  get(target, propKey) {
    return 'abc';
  }
};

const proxy = new Proxy(target, handler);

proxy.foo
//Uncaught TypeError
```

### 2. set()

`set`方法用来拦截某个属性的赋值操作，可以接受四个参数，依次为目标对象、属性名、属性值和 Proxy 实例本身（可选）。

返回一个布尔值，返回true代表此次设置属性成功了，如果返回false且设置属性操作发生在严格模式下，那么会抛出一个`TypeError`。

```js
let obj = {}
let proxy = new Proxy(obj,{
    set:function(target,property,value,proxy){
        console.log("修改对象属性")
    }
})
proxy.name = 'name'	//修改对象属性
obj		//{name: "name"}
proxy	//Proxy {name: "name"}
```

有时，我们会在对象上面设置内部属性，属性名的第一个字符使用下划线开头，表示这些属性不应该被外部使用。结合`get`和`set`方法，就可以做到防止这些内部属性被外部读写。

```js
const tom = {
    _id:'Tom'
}
let proxy = new Proxy(tom,{
    get:function(target,property,pro){
        property.startsWith('_') && throw new ReferenceError(`Can not read property ${property}`)
        return target[property]
    },
    set:function(target,property,value,pro){
        property.startsWith('_') && throw new ReferenceError(`Can not set property ${property}`)
        target[property] = value
    }
})
proxy._id	//Uncaught ReferenceError: Can not read property _id
proxy._age = 20	//Uncaught ReferenceError: Can not set property _age
proxy.name = 'Tom'	//修改成功
proxy	//Proxy {_id: "123456", name: "Tom"}
tom		//{_id: "123456", name: "Tom"}
```

**注意：**如果目标对象自身的某个属性，不可写且不可配置，那么`set`方法将不起作用。严格模式下，`set`代理如果没有返回`true`，就会报错。

### 3. apply()

`apply`方法拦截函数的调用、`call`和`apply`操作。可接受三个参数，分别是目标对象（函数）、目标对象的上下文对象（`this`）和目标对象的参数数组。

可以返回任何值。

```js
let fn = function(){
    console.log(this)
}
let proxy = new Proxy(fn,{
    apply:function(target,context,args){
        console.log(target)
        console.log(context)
        console.log(args)
    }
})
proxy()
/**
 * fn(){}
 * undifiend
 * []
 */
proxy.call({foo:"foo"},1,2,3)
/**
 * fn(){}
 * {foo:"foo"}
 * [1,2,3]
 */
proxy.apply({foo:"foo"},[1,2,3])
/**
 * fn(){}
 * {foo:"foo"}
 * [1,2,3]
 */
```

### 4. has()

`has`方法用来拦截`HasProperty`操作，即判断对象是否具有某个属性时，这个方法会生效，可以接受两个参数，分别是目标对象、需查询的属性名。**典型的操作就是`in`运算符**。

返回一个布尔值。

```js
let handler = {
    has:function(target,property){
        console.log(target)
        console.log(property)
        return property in target
    }
}
let proxy = new Proxy({foo:"foo"},handler)
"foo" in proxy
/**
 * {foo:"foo"}
 * foo
 * true
 */
```

如果原对象不可配置或者禁止扩展，这时`has`方法就不得“隐藏”（即返回`false`）目标对象的该属性。

```js
let obj = { foo:"foo" }
Object.preventExtensions(obj)
let proxy = new Proxy(obj,{
    has:function(target,property){
        return false
    }
})
'foo' in proxy	//Uncaught TypeError
```

**注意：**`has`方法拦截的是`HasProperty`操作，而不是`HasOwnProperty`操作，即`has`方法不判断一个属性是对象自身的属性，还是继承的属性。另外，虽然`for...in`循环也用到了`in`运算符，但是`has`拦截对`for...in`循环不生效。

### 5. construct()

`construct`方法用于拦截`new`命令。

`construct`方法可以接受两个参数：

- `target`：目标对象
- `args`：构造函数的参数对象

必须返回一个对象。

```js
let Fn = function(name){
        this.name = name
    }
let proxy = new Proxy(Fn,{
    construct:function(target,args){
        console.log(target)	//Fn(name){}
        console.log(args)	//["Tom"]
        return new target(...args)
    }
})
let f = new proxy("Tom")
f	//{name: "Tom"}
```

### 6. deleteProperty()

`deleteProperty`方法用于拦截`delete`操作，如果这个方法抛出错误或者返回`false`，当前属性就无法被`delete`命令删除。

```js
let obj = { foo:"foo" }
let proxy = new Proxy(obj,{
    deleteProperty:function(target,key){
        throw new syntaxError(`Can not delete ${property} of ${target}`)
        /**
         * return delete target[key]
         */
    }
})
delete obj.foo	//Uncaught SyntaxError: Can not delete foo of [object Object]
```

### 7. defineProperty()

`defineProperty`方法拦截了`Object.defineProperty`操作。**若返回fasle，则会导致新属性总是添加无效**。

接收三个参数：

+ 目标对象
+ 被定义的属性名
+ 属性描述对象

```js
let obj = {}
let proxy = new Proxy(obj,{
    defineProperty(target,property,descriptor){
        //target[property] = descriptor.value
        return false
    }
})
obj.name = 'name'
obj	//{}
Object.defineProperty(proxy,foo,{	//Uncaught TypeError
    value:'foo'
})
```

### 8. getOwnPropertyDescriptor()

`getOwnPropertyDescriptor`方法拦截`Object.getOwnPropertyDescriptor()`，返回一个属性描述对象或者`undefined`。

接收两个参数：

+ 目标对象
+ 属性名

必须返回一个 object 或 `undefined`。

```js
let obj = { foo:"foo" }
let proxy =  new Proxy(obj,{
    getOwnPropertyDescriptor(target,property){
        console.log(target)
        console.log(property)
        return Object.getOwnPropertyDescriptor(target,property)
    }
})
Object.getOwnPropertyDescriptor(proxy,foo)
//{value: "foo", writable: true, enumerable: true, configurable: true}
```

### 9. getPrototypeOf()

`getPrototypeOf`方法主要用来拦截获取对象原型。具体来说，拦截下面这些操作：

- `Object.prototype.__proto__`
- `Object.prototype.isPrototypeOf()`
- `Object.getPrototypeOf()`
- `Reflect.getPrototypeOf()`
- `instanceof`

接收一个目标对象作为参数。

返回值必须是对象或者null，如果目标对象不可扩展（non-extensible）， `getPrototypeOf`方法必须返回目标对象的原型对象。

```js
let obj = {}
//Object.preventExtensions(obj)
let proxy = new Proxy(obj,{
    getPrototypeOf(target){
        //return target.__proto__
        return target
    }
})
proxy.__proto__	//{}
```

### 10. isExtensible()

`isExtensible`方法拦截`Object.isExtensible`操作。

接收一个目标对象作为参数。

必须返回一个 Boolean值或可转换成Boolean的值。

```js
let obj = {}
Object.preventExtensions(obj)
let proxy = new Proxy(obj,{
    isExtensible(target){
        console.log(Object.isExtensible)
        return Object.isExtensible(target)
    }
})
Object.isExtensible(proxy)	//false
```

**注意：**`Object.isExtensible(proxy)` 必须同`Object.isExtensible(target)`返回相同值。也就是必须返回true或者为true的值,返回false和为false的值都会报错。

### 11. ownKeys()

`ownKeys`方法用来拦截对象自身属性的读取操作。具体来说，拦截以下操作：

- `Object.getOwnPropertyNames()`
- `Object.getOwnPropertySymbols()`
- `Object.keys()`
- `for...in`循环

接受一个目标对象作为参数。

必须返回一个可枚举对象，且成员必须是字符串或Symbol值。

```js
let obj = {
    foo:"foo",
    bar:"bar",
    hide:"hide"
}
Object.defineProperty(obj,'hide',{
    enumerable:false
})
let proxy = new Proxy(obj,{
    ownKeys(target){
        return Object.keys(target)
        //return ['foo','bar','hide']	//这里的字符串对应obj中的键名
    }
})
Object.keys(proxy)	//["foo", "bar"]
//Object.getOwnPropertyNames(proxy)	//["foo", "bar"]
```

**注意：**如果目标对象是不可扩展的（non-extensible），这时`ownKeys`方法返回的数组之中，必须包含原对象的所有属性，且不能包含多余的属性。

### 12. preventExtensions()

`preventExtensions`方法拦截`Object.preventExtensions()`。该方法必须返回一个布尔值，否则会被自动转为布尔值。

接受一个目标对象作为参数。

返回一个布尔值，只有目标对象不可扩展时（即`Object.isExtensible(proxy)`为`false`），`proxy.preventExtensions`才能返回`true`，否则会报错。

```js
let obj = {}
let  proxy = new Proxy(obj,{
    preventExtensions(target){
        Object.preventExtensions(target)
        return true
    }
})
Object.isExtensible(proxy)
```

### 13. setPrototypeOf()

`setPrototypeOf`方法主要用来拦截`Object.setPrototypeOf`方法。

接受两个参数：

+ 目标对象
+ 对象原型或null

如果成功修改了`[[Prototype]]`, `setPrototypeOf` 方法返回 `true`,否则返回 `false`

```js
let obj = {}
let proxy = new Proxy(obj,{
    setPrototypeOf(target,proto){
        Object.setPrototypeOf(target,proto)
        return true
    }
})
let proto = {foo:"foo"}
Object.setPrototypeOf(proxy,proto)
obj.__proto__
```

**注意：**如果目标对象不可扩展（non-extensible），`setPrototypeOf`方法不得改变目标对象的原型。

---

## Proxy.revocable()

`Proxy.revocable`方法返回一个可取消的 Proxy 实例。

返回一个对象，该对象的`proxy`属性是`Proxy`实例，`revoke`属性是一个函数，可以取消`Proxy`实例。

```js
let target = {}
let handler = {}
let { proxy, revoke } = Proxy.revocable(target, handler)
proxy.foo = "foo"
proxy.foo	//foo
revoke()
proxy.foo	//Uncaught TypeError
```

**应用场景：**目标对象不允许直接访问，必须通过代理访问，一旦访问结束，就收回代理权，不允许再次访问。

## Proxy代理的this问题

```js
const obj = {
    test(){
        console.log(this === proxy)
    }
}
const proxy = new Proxy(obj,{
    get(target,property){
        return target[property]
    }
})
obj.test()		//false
proxy.test()	//true
```

**注意：**由上面的例子可以看出，在 Proxy 代理的情况下，目标对象内部的`this`关键字会指向 Proxy 代理。此外，有些原生对象的内部属性，只有通过正确的`this`才能拿到，所以 Proxy 也无法代理这些原生对象的属性。

```js
const target = new Date();
const handler = {};
const proxy = new Proxy(target, handler);

proxy.getDate();
//TypeError: this is not a Date object.
```

上面代码中，`getDate`方法只能在`Date`对象实例上面拿到，如果`this`不是`Date`对象实例就会报错。这时，`this`绑定原始对象，就可以解决这个问题。

```js
const target = new Date('2015-01-01');
const handler = {
  get(target, prop) {
    if (prop === 'getDate') {
      return target.getDate.bind(target);
    }
    return Reflect.get(target, prop);
  }
};
const proxy = new Proxy(target, handler);

proxy.getDate()
```

