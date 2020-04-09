# ES6-Set和Map数据结构

## Set

### 基本用法

它类似于数组，但是成员的值都是**唯一的**，**没有重复的**值。`Set`本身是一个构造函数，用来生成 Set 数据结构。

```js
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

`Set`函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

```js
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 去除数组、字符串的重复成员
[...new Set(array)]
[...new Set(string)].join('')
```

### Set实例的属性和方法

#### 属性

+ `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
+ `Set.prototype.size`：返回`Set`实例的成员总数。

```js
let s = new Set([1,2,3])
s	//Set(3) {1, 2, 3}
s.constructor	//Set(){...}
s.size	//3
```

#### 方法

+ `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。

+ `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。

+ `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。

+ `Set.prototype.clear()`：清除所有成员，没有返回值。

  ```js
  let s = new Set()
  s.add(1)	//Set(1) {1}
  s.add(2)	//Set(2) {1,2}
  s.delete(1)	//Set(1) {2}
  s.has(1)	//false
  s.has(2)	//true
  s.clear()	//Set(0) {}
  ```

### 遍历

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- `Set.prototype.keys()`：返回键名的遍历器
- `Set.prototype.values()`：返回键值的遍历器
- `Set.prototype.entries()`：返回键值对的遍历器
- `Set.prototype.forEach()`：使用回调函数遍历每个成员

#### keys(), values(), entries()

由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以`keys`方法和`values`方法的行为完全一致。

```js
let s = new Set([1,2,3,4,5])
s.keys()	//SetIterator {1, 2, 3, 4, 5}
s.values()	//SetIterator {1, 2, 3, 4, 5}
s.entries()	//SetIterator {1 => 1, 2 => 2, 3 => 3, 4 => 4, 5 => 5}
```

Set 结构的实例默认可遍历，它的默认遍历器生成函数就是它的`values`方法。

```js
let s = new Set(['a','b','c','d'])
for(let val of s){	//相当于let val of s.values()
    console.log(val)	//a  b  c  d
}
```

#### forEach()

Set 结构的实例与数组一样，也拥有`forEach`方法，用于对每个成员执行某种操作，没有返回值。

```js
let s = new Set(['a','b','c','d'])
s.forEach((el,key,set) => {
    log(el)	//a  b  c  d
}[,this])
```

#### 遍历的应用

扩展运算符和 Set 结构相结合，去除数组的重复成员。

```js
let arr = [1,1,2,3,3,5,7,7]
arr = [...new Set(arr)]
```

数组的`map`和`filter`方法也可以间接用于 Set 

```js
let s = new Set([12,3,4,5])
//map
[...s].map(el => el * 2)	//[24, 6, 8, 10]
//filter
[...s].filter(el => (el % 2) == 0)	//[12, 4]
```

Set 可以很容易地实现并集（Union）、交集（Intersect）

```js
let s1 = new Set([1,2,4,6,7])
let s2 = new Set([2,4,6,8,9])
//并集
new Set([...s1,...s2])	//Set(7) {1, 2, 4, 6, 7, 8, 9}
//交集
new Set([...s1].filter(el => s2.has(el)))	//Set(3) {2, 4, 6}
```

在遍历操作中改变原来的 Set 结构

```js
//方式一
let s = new Set([2,4,6])
s = new Set([...s].map(el => el * 2))	//Set(3) {4, 8, 12}
//方式二
s = new Set(Array.from(s,val => val * 2))	//Set(3) {4, 8, 12}
```

## WeakSet

WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别：

+ WeakSet 的成员只能是对象，而不能是其他类型的值。

+ WeakSet 中的对象都是弱引用

  > **弱引用：**如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象是否还在该弱引用的结构中。

WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。由于这个特点，WeakSet 的成员是不适合引用的，因为它会随时消失。另外，由于 WeakSet 内部有多少个成员，取决于垃圾回收机制有没有运行，运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 ES6 规定 WeakSet 不可遍历。

### 基本用法

WeakSet 是一个构造函数，可以使用`new`命令，创建 WeakSet 数据结构。
作为构造函数，WeakSet 可以接受一个数组或类似数组的对象作为参数。（实际上，任何具有 Iterable 接口的对象，都可以作为 WeakSet 的参数。）**该数组对象的所有成员**（数组的成员只能是对象），都会自动成为 WeakSet 实例对象的成员。

```js
const a = [[1, 2], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2], [3, 4]}

const b = [3, 4];
const ws = new WeakSet(b);
//Uncaught TypeError: Invalid value used in weak set
```

### 内置方法

+ **WeakSet.prototype.add(value)**：向 WeakSet 实例添加一个新成员。
+ **WeakSet.prototype.delete(value)**：清除 WeakSet 实例的指定成员。
+ **WeakSet.prototype.has(value)**：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。

```js
const ws = new WeakSet();
const obj = {};
const foo = {};

ws.add(window);
ws.add(obj);

ws.has(window); // true
ws.has(foo);    // false

ws.delete(window);
ws.has(window);    // false
```

WeakSet 的另一个例子

```js
const foos = new WeakSet()
class Foo {
  constructor() {
    foos.add(this)
  }
  method () {
    if (!foos.has(this)) {
      throw new TypeError('Foo.prototype.method 只能在Foo的实例上调用！');
    }
  }
}
```

上面代码保证了`Foo`的实例方法，只能在`Foo`的实例上调用。这里使用 WeakSet 的好处是，`foos`对实例的引用，不会被计入内存回收机制，所以删除实例的时候，不用考虑`foos`，也不会出现内存泄漏。

## Map

### 基本用法

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

```js
const data = {};
const element = document.getElementById('myDiv');

data[element] = 'metadata';
data['[object HTMLDivElement]'] // "metadata"
//上面代码原意是将一个 DOM 节点作为对象data的键，但是由于对象只接受字符串作为键名，所以element被自动转为字符串[object HTMLDivElement]。
```

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是**“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键**。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

```js
const m = new Map()
const obj = {foo:'foo'}
m.set(obj,'something')	//将对象obj当做m的一个键
m.get(obj)	//something
m.has(obj)	//true
m.delete(obj)	//true
m.has(obj)	//false
```

作为构造函数，Map 也可以接受一个数组作为参数。该数组的**成员是一个个表示键值对的数组**。

```js
let obj = {aaa:'aaa'}
const m = new Map([
    ["foo","foo"],
    ["bar","bar"],
    [obj,obj]
])
m	//Map(3) {"foo" => "foo", "bar" => "bar", {…} => {…}}
m.get(obj)	//{aaa: "aaa"}
```

任何具有 Iterator 接口、且每个成员都是一个双元素的数组的数据结构都可以当作`Map`构造函数的参数

```js
const set = new Set([
    ["foo",'foo'],
    ['bar','bar']
])
const map = new Map(set)
map	//Map(2) {"foo" => "foo", "bar" => "bar"}
```

### Map实例的属性和方法

1. **size属性**

   `size`属性返回 Map 结构的成员总数。

2. **Map.prototype.set(key, value)**

   `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。`set`方法返回的是当前的`Map`对象，因此可以采用链式写法。

   ```js
   const m = new Map()
   m.set('foo','foo')
   m	//Map(1) {"foo" => "foo"}
   m.set('foo','bar')
   m	//Map(1) {"foo" => "bar"}
   ```

3. **Map.prototype.get(key)**

   `get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。

4. **Map.prototype.has(key)**

   `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

5. **Map.prototype.delete(key)**

   `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。

6. **Map.prototype.clear()**

   `clear`方法清除所有成员，没有返回值。

### Map结构的遍历

Map 结构原生提供三个遍历器生成函数和一个遍历方法：

```js
const m = new Map([
    ['foo','foo'],
    ['bar','bar'],
])
```

- `Map.prototype.keys()`：返回键名的遍历器。

  ```js
  m.keys()	//MapIterator {"foo", "bar"}
  ```

- `Map.prototype.values()`：返回键值的遍历器。

  ```js
  m.values()	//MapIterator {"foo", "bar"}
  ```

- `Map.prototype.entries()`：返回所有成员的遍历器。

  ```js
  m.entries()	//MapIterator {"foo" => "foo", "bar" => "bar"}
  ```

- `Map.prototype.forEach()`：遍历 Map 的所有成员。

  ```js
  m.forEach((value,key,map) => {
      console.log(el)
  })
  //foo foo
  //bar bar
  ```

## WeakMap

`WeakMap`结构与`Map`结构类似，也是用于生成键值对的集合，但和Map有两点区别：

+ `WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名。

  ```js
  const wmap = new WeakMap();
  wmap.set(1, 2)
  // TypeError: 1 is not an object!
  wmap.set(Symbol(), 2)
  // TypeError: Invalid value used as weak map key
  wmap.set(null, 2)
  // TypeError: Invalid value used as weak map key
  ```

+ `WeakMap`的键名所指向的对象，不计入垃圾回收机制（弱引用）。

`WeakMap`的专用场合就是，它的键所对应的对象，可能会在将来消失。`WeakMap`结构有助于防止内存泄漏。**注意：**WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。

### WeakMap的语法

WeakMap 与 Map 在 API 上的区别主要是两个，一是没有遍历操作（即没有`keys()`、`values()`和`entries()`方法），也没有`size`属性。可用方法：

1. **WeakMap.prototype.set()**
2. **WeakMap.prototype.get()**
3. **WeakMap.prototype.delete()**
4. **WeakMap.prototype.has()**