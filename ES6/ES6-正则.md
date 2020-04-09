# 正则

## RegExp构造函数

> ES5中，`RegExp`构造函数的参数有两种情况
>
> ```js
> //情况一
> var regex = new RegExp('xyz', 'i');
> 	// 等价于
> var regex = /xyz/i;
> 
> //情况二
> var regex = new RegExp(/xyz/i);
> 	// 等价于
> var regex = /xyz/i;
> 
> //但不允许以下行为
> var regex = new RegExp(/xyz/, 'i');
> ```

ES6中，如果`RegExp`构造函数第一个参数是一个正则对象，那么可以使用第二个参数指定修饰符，返回的正则表达式会忽略原有的正则表达式的修饰符，只使用新指定的修饰符。

```js
var regex = new RegExp(/abc/ig,'i')
//regex = /abc/i
```

## 字符串方法

字符串对象共有 4 个方法，可以使用正则表达式：`match()`、`replace()`、`search()`和`split()`。ES6 将这 4 个方法，在语言内部全部调用`RegExp`的实例方法，从而做到所有与正则相关的方法，全都定义在`RegExp`对象上。

> `String.prototype.match` 调用 `RegExp.prototype[Symbol.match]`
>
> `String.prototype.replace` 调用 `RegExp.prototype[Symbol.replace]`
>
> `String.prototype.search` 调用 `RegExp.prototype[Symbol.search]`
>
> `String.prototype.split` 调用 `RegExp.prototype[Symbol.split]`

## u修饰符

含义为“Unicode 模式”，用来正确处理大于`\uFFFF`的 Unicode 字符。也就是说，会正确处理四个字节的 UTF-16 编码。

一旦加上u修饰符，以下正则表达式的行为就会被修改。

+ **点字符**

  对于码点大于`0xFFFF`的 Unicode 字符，点字符不能识别，必须加上`u`修饰符。如果不添加`u`修饰符，正则表达式就会认为字符串为两个字符，从而匹配失败。

+ **Unicode 字符表示法**

  ES6 新增了使用大括号表示 Unicode 字符，这种表示法在正则表达式中必须加上`u`修饰符，才能识别当中的大括号，否则会被解读为量词。

  ```js
  /\u{61}/.test('a') // false
  /\u{61}/u.test('a') // true
  /\u{20BB7}/u.test('𠮷') // true
  ```

+ **量词**

  使用`u`修饰符后，所有量词都会正确识别码点大于`0xFFFF`的 Unicode 字符。

  ```js
  /a{2}/.test('aa') // true
  /a{2}/u.test('aa') // true
  /𠮷{2}/.test('𠮷𠮷') // false
  /𠮷{2}/u.test('𠮷𠮷') // true
  ```

+ **预定义模式**

  `u`修饰符也影响到预定义模式，能否正确识别码点大于`0xFFFF`的 Unicode 字符。

+ **i 修饰符**

  有些 Unicode 字符的编码不同，但是字型很相近，比如，`\u004B`与`\u212A`都是大写的`K`。

  ```js
  /[a-z]/i.test('\u212A') // false
  /[a-z]/iu.test('\u212A') // true
  //上面代码中，不加u修饰符，就无法识别非规范的K字符。
  ```

+ **转义**

  没有`u`修饰符的情况下，正则中没有定义的转义（如逗号的转义`\,`）无效，而在`u`模式会报错。

## RegExp.prototype.unicode 属性

正则实例对象新增`unicode`属性，表示是否设置了`u`修饰符。

```js
const r1 = /hello/;
const r2 = /hello/u;

r1.unicode // false
r2.unicode // true
```

## y 修饰符

“粘连”（sticky）修饰符。`y`修饰符的作用与`g`修饰符类似，也是全局匹配，后一次匹配都从上一次匹配成功的下一个位置开始。`g`修饰符只要剩余位置中存在匹配就可，而`y`修饰符确保**匹配必须从剩余的第一个位置开始**。

```js
var s = 'aaa_aa_a';
var r1 = /a+/g;
var r2 = /a+/y;

r1.exec(s) // ["aaa"]
r2.exec(s) // ["aaa"]

r1.exec(s) // ["aa"]
r2.exec(s) // null
```

使用`lastIndex`属性，可以更好地说明`y`修饰符。`lastIndex`属性指定每次搜索的开始位置，**`g`修饰符从这个位置开始向后搜索，直到发现匹配为止**。`y`修饰符同样遵守`lastIndex`属性，但是要求**必须在`lastIndex`指定的位置发现匹配**。

```js
const REGEX = /a/y;

// 指定从2号位置开始匹配
REGEX.lastIndex = 2;

// 不是粘连，匹配失败
REGEX.exec('xaya') // null

// 指定从3号位置开始匹配
REGEX.lastIndex = 3;

// 3号位置是粘连，匹配成功
const match = REGEX.exec('xaya');
match.index // 3
REGEX.lastIndex // 4
```

`y`修饰符的设计本意，就是让头部匹配的标志`^`在全局匹配中都有效。但是`y`修饰符对`match`方法，只能返回第一个匹配，必须与`g`修饰符联用，才能返回所有匹配。

## RegExp.prototype.sticky属性

ES6 的正则实例对象多了`sticky`属性，表示是否设置了`y`修饰符。

```js
var r = /hello\d/y;
r.sticky // true
```

## RegExp.prototype.flags 属性

ES6 为正则表达式新增了`flags`属性，会返回正则表达式的修饰符。

```js
// ES5 的 source 属性
// 返回正则表达式的正文
/abc/ig.source
// "abc"

// ES6 的 flags 属性
// 返回正则表达式的修饰符
/abc/ig.flags
// 'gi'
```

## s 修饰符：dotAll 模式

ES2018 引入s修饰符，使得`.`可以匹配任意单个字符，这被称为`dotAll`模式，即点（dot）代表一切字符（包括换行符）。

## 后行断言

> **先行断言：**`x`只有在`y`前面才匹配，必须写成`/x(?=y)/`，只匹配百分号之前的数字，要写成`/\d+(?=%)/`。“先行断言”括号之中的部分（`(?=%)`），是不计入返回结果的。
>
> ```js
> /\d+(?=%)/.exec('100% of US presidents have been male')  // ["100"]
> /\d+(?!%)/.exec('that’s all 44 of them')                 // ["44"]
> ```
>
> **先行否定断言：**`x`只有不在`y`前面才匹配，必须写成`/x(?!y)/`，比如，只匹配不在百分号之前的数字，要写成`/\d+(?!%)/`。

“后行断言”正好与“先行断言”相反，`x`只有在`y`后面才匹配，必须写成`/(?<=y)x/`。比如，只匹配美元符号之后的数字，要写成`/(?<=\$)\d+/`。
“后行否定断言”则与“先行否定断言”相反，`x`只有不在`y`后面才匹配，必须写成`/(?<!y)x/`。比如，只匹配不在美元符号后面的数字，要写成`/(?<!\$)\d+/`。

```js
/(?<=\$)\d+/.exec('Benjamin Franklin is on the $100 bill')  // ["100"]
/(?<!\$)\d+/.exec('it’s is worth about €90')                // ["90"]
```

> 正则匹配的贪婪模式：在满足匹配规则的条件下尽可能多的匹配。

“后行断言”的反斜杠引用，也与通常的顺序相反，必须放在对应的那个括号之前。因为后行断言是先从左到右扫描，发现匹配以后再回过头，从右到左完成反斜杠引用。

> 反斜杠引用( \n )：对第n个分组括号里的匹配项进行一次引用

```js
/(?<=(o)d\1)r/.exec('hodor')  // null
/(?<=\1d(o))r/.exec('hodor')  // ["r", "o"]
```

## Unicode 属性类

ES2018 引入了一种新的类的写法`\p{...}`和`\P{...}`，允许正则表达式匹配符合 Unicode 某种属性的所有字符。

```js
const regexGreekSymbol = /\p{Script=Greek}/u;
regexGreekSymbol.test('π') // true
//\p{Script=Greek}指定匹配一个希腊文字母，所以匹配π成功
```

由于 Unicode 的各种属性非常多，所以这种新的类的表达能力非常强。

```js
\p{UnicodePropertyName=UnicodePropertyValue}
//对于某些属性，可以只写属性名，或者只写属性值
\p{UnicodePropertyName}
\p{UnicodePropertyValue}
```

## 具名组匹配

“具名组匹配”在分组圆括号内部，模式的头部添加“问号 + 尖括号 + 组名”（`?<year>`），然后就可以在`exec`方法返回结果的`groups`属性上引用该组名。

```js
const RE_DATE = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;

const matchObj = RE_DATE.exec('1999-12-31');
const year = matchObj.groups.year; // 1999
const month = matchObj.groups.month; // 12
const day = matchObj.groups.day; // 31
```

### 解构赋值和替换

有了具名组匹配以后，可以使用解构赋值直接从匹配结果上为变量赋值。

```js
var {groups:{
    one,
    two
}} = /(?<one>.*):(?<two>.*)/g.exec('foo:bar')
//one:'foo'		two:'bar'
```

字符串替换时，使用`$<组名>`引用具名组。

```js
let re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/u;
'2015-01-02'.replace(re, '$<day>/$<month>/$<year>')		// '02/01/2015'

'2015-01-02'.replace(re, (
   matched, // 整个匹配结果 2015-01-02
   capture1, // 第一个组匹配 2015
   capture2, // 第二个组匹配 01
   capture3, // 第三个组匹配 02
   position, // 匹配开始的位置 0
   S, // 原字符串 2015-01-02
   groups // 具名组构成的一个对象 {year, month, day}
 ) => {
 let {day, month, year} = groups;
 return `${day}/${month}/${year}`;
});
```

### 引用

如果要在正则表达式内部引用某个“具名组匹配”，可以使用`\k<组名>`的写法。

```js
let reg = /(?<one>\d+)-\k<one>$/g
//数字引用依然有效
let reg = /(?<one>\d+)-\1$/g
//这两种引用语法还可以同时使用
let reg = /(?<one>\d+)-\1\k<one>$/g
```

## String.prototype.matchAll

可以一次性取出所有匹配。不过，它返回的是一个遍历器（Iterator），而不是数组。

```js
const string = 'test1test2test3';

// g 修饰符加不加都可以
const regex = /t(e)(st(\d?))/g;

for (const match of string.matchAll(regex)) {
  console.log(match);
}
// ["test1", "e", "st1", "1", index: 0, input: "test1test2test3"]
// ["test2", "e", "st2", "2", index: 5, input: "test1test2test3"]
// ["test3", "e", "st3", "3", index: 10, input: "test1test2test3"]
```

**遍历器转为数组：**

```js
// 转为数组方法一，使用'...'运算符'
[...string.matchAll(regex)]

// 转为数组方法二，使用Array.from()
Array.from(string.matchAll(regex));
```

