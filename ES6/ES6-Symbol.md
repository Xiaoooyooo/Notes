# ES6-Symbol

ES6 引入了一种新的原始数据类型`Symbol`，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：`undefined`、`null`、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

Symbol 值通过`Symbol`函数生成。这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。**凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突**。

`Symbol`函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

```js
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```

## Symbol.prototype.description

创建 Symbol 的时候，可以添加一个描述，往 Symbol 函数传入一个字符串即可。该方法可以获取到 Symbol 的描述。

```js
let s = Symbol()
let s1 = Symbol('s1')
s.description //undefined
s1.description //s1
```

## 作为属性名的 Symbol

由于每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。

```js
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

## 作为属性名的Symbol的遍历

Symbol 作为属性名，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。

`Object.getOwnPropertySymbols`方法，可以获取指定对象的所有 Symbol 属性名。`Object.getOwnPropertySymbols`方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。

```js
let s1 = Symbol("c")
let s2 = Symbol("d")
let obj = {
    a:'aa',
    b:'bb',
    [s1]:"cc",
    [s2]:'dd'
}
for(let key in obj){
    console.log(key)	//a b
}
Object.getOwnPropertySymbols(obj)	//[Symbol(c), Symbol(d)]
```

## Symbol.for()，Symbol.keyFor()

+ **Symbol.for()**

  它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建并返回一个以该字符串为名称的 Symbol 值。

	**注意：`Symbol.for`为 Symbol 值登记的名字，是全局环境的**，可以在不同的 iframe 或 service worker 中取到同一个值。
	
	```js
	let s = Symbol('s')
Symbol.for('s') === s	//false
	
	let s1 = Symbol.for('foo')
	let s2 = Symbol.for("foo")
	s1 === s2	//true
	```

+ **Symbol.keyFor()**

  `Symbol.keyFor`方法返回一个已登记的 Symbol 类型值的`key`。

  ```js
  let s1 = Symbol.for("foo");
  Symbol.keyFor(s1) // "foo"
  
  let s2 = Symbol("foo");
  Symbol.keyFor(s2) // undefined
  //上面的s2属于未登记的Symbol值，所以返回undefined
  ```

## 内置的Symbol值

1. **Symbol.hasInstance**

   对象的`Symbol.hasInstance`属性，指向一个内部方法。当其他对象使用`instanceof`运算符，判断是否为该对象的实例时，会调用这个方法。比如，`foo instanceof Foo`在语言内部，实际调用的是`Foo[Symbol.hasInstance](foo)`。

   ```js
   let a = {
       [Symbol.hasInstance](val){
           return val < 10
       }
   }
   let b = 5,
       c = 11
   b instanceof a	//true
   //等同于	a[Symbol.hasInstance](b)
   c instanceof a	//false
   ```

2. **Symbol.isConcatSpreadable**

   对象的`Symbol.isConcatSpreadable`属性等于一个布尔值，表示该对象用于`Array.prototype.concat()`时，是否可以展开。

   ```js
   let arr1 = ['c', 'd'];
   ['a', 'b'].concat(arr1, 'e') // ['a', 'b', 'c', 'd', 'e']
   arr1[Symbol.isConcatSpreadable] // undefined
   
   let arr2 = ['c', 'd'];
   arr2[Symbol.isConcatSpreadable] = false;	//将arr2中的Symbol.isConcatSpreadable值修改为false
   ['a', 'b'].concat(arr2, 'e') // ['a', 'b', ['c','d'], 'e']
   ```

   类似数组的对象正好相反，默认不展开。它的`Symbol.isConcatSpreadable`属性设为`true`，才可以展开。

   ```js
   let obj = {length: 2, 0: 'c', 1: 'd'};
   ['a', 'b'].concat(obj, 'e') // ['a', 'b', obj, 'e']
   
   obj[Symbol.isConcatSpreadable] = true;
   ['a', 'b'].concat(obj, 'e') // ['a', 'b', 'c', 'd', 'e']
   ```

   `Symbol.isConcatSpreadable`属性也可以定义在类里面。

   ```js
   class A1 extends Array {
     constructor(args) {
       super(args);
       this[Symbol.isConcatSpreadable] = true;	//定义在类的实例上
     }
   }
   class A2 extends Array {
     constructor(args) {
       super(args);
     }
     get [Symbol.isConcatSpreadable] () {
       return false;	//定义在类本身上
     }
   }
   ```

3. **Symbol.species**

   对象的`Symbol.species`属性，指向一个构造函数。**创建衍生对象时，会使用该属性**。

   ```js
   class MyArray extends Array {
       //static get [Symbol.species]() { return Array; }
   }
   
   const a = new MyArray(1, 2, 3);
   const b = a.map(x => x);
   const c = a.filter(x => x > 1);
   
   b instanceof MyArray // true
   c instanceof MyArray // true
   //如果取消MyArray中的注释则会是以下结果
   b instanceof MyArray // false
   c instanceof MyArray // false
   ```

4. **Symbol.match**

   对象的`Symbol.match`属性，指向一个函数。当执行`str.match(myObject)`时，如果该属性存在，会调用它，返回该方法的返回值。

5. **Symbol.replace**

   对象的`Symbol.replace`属性，指向一个方法，当该对象被`String.prototype.replace`方法调用时，会返回该方法的返回值。

6. **Symbol.search**

   对象的`Symbol.search`属性，指向一个方法，当该对象被`String.prototype.search`方法调用时，会返回该方法的返回值。

7. **Symbol.split**

   对象的`Symbol.split`属性，指向一个方法，当该对象被`String.prototype.split`方法调用时，会返回该方法的返回值。

8. **Symbol.iterator**

   对象的`Symbol.iterator`属性，指向该对象的默认遍历器方法。

9. **Symbol.toPrimitive**

   对象的`Symbol.toPrimitive`属性，指向一个方法。该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。

10. **Symbol.toStringTag**

    对象的`Symbol.toStringTag`属性，指向一个方法。在该对象上面调用`Object.prototype.toString`方法时，如果这个属性存在，它的返回值会出现在`toString`方法返回的字符串之中，表示对象的类型。

11. **Symbol.unscopables**

    对象的`Symbol.unscopables`属性，指向一个对象。该对象指定了使用`with`关键字时，哪些属性会被`with`环境排除。

