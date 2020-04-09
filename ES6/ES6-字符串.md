# ES6-字符串

## 字符串的遍历器接口

+ ES6 为字符串添加了遍历器接口，使得字符串可以被`for...of`循环遍历

  ```js
  for (let codePoint of 'foo') {
    console.log(codePoint)
  }
  // "f" "o" "o"
  ```

## 标签模板

它可以紧跟在一个函数名后面，该函数将被调用来处理这个模板字符串。这被称为“标签模板”功能。

```js
alert`123`
// 等同于
alert(123)
```

标签模板其实不是模板，而是函数调用的一种特殊形式。“标签”指的就是函数，紧跟在后面的模板字符串就是它的参数。但是，如果模板字符里面有变量，就不是简单的调用了，而是会将模板字符串先处理成多个参数，再调用函数。
**如此调用的函数的第一个参数是一个数组**，该数组的成员是模板字符串中那些没有变量替换的部分，**也就是说，变量替换只发生在数组的第一个成员与第二个成员之间、第二个成员与第三个成员之间**。
**其他参数，都是模板字符串各个变量被替换后的值**

```js
let a = 5;
let b = 10;

tag`Hello ${ a + b } world ${ a * b }`;
// 等同于
tag(['Hello ', ' world ', ''], 15, 50);
```

## 字符串新增方法

+ **String.fromCodePoint()**

  > ES5 提供`String.fromCharCode()`方法，用于从 Unicode 码点返回对应字符，但是这个方法不能识别码点大于`0xFFFF`的字符。
  >
  > ```js
  > String.fromCharCode(0x20BB7)
  > // "ஷ"	=>	U+0BB7
  > ```

  ES6 提供了`String.fromCodePoint()`方法，可以识别大于`0xFFFF`的字符，弥补了`String.fromCharCode()`方法的不足。在作用上，正好与codePointAt()方法相反。

  ```js
  String.fromCodePoint(0x20BB7)
  // "𠮷"
  
  //如果`String.fromCodePoint`方法有多个参数，则它们会被合并成一个字符串返回。
  String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y'
  // true
  ```

  **注意：**`fromCodePoint`方法定义在`String`对象上，而`codePointAt`方法定义在字符串的实例对象上。

+ **String.raw()**

  用于获取模板字符串的**原始字符串形式**。

  ```js
  String.raw`Hi\n${2+3}!`
  // 实际返回 "Hi\\n5!"，显示的是转义后的结果 "Hi\n5!"
  
  String.raw`Hi\u000A!`;
  // 实际返回 "Hi\\u000A!"，显示的是转义后的结果 "Hi\u000A!"
  ```

+ **实例方法**

  + **codePointAt()**
  
    JavaScript 内部，字符以 UTF-16 的格式储存，每个字符固定为`2`个字节。对于那些需要`4`个字节储存的字符（Unicode 码点大于`0xFFFF`的字符），JavaScript 会认为它们是两个字符。
  
    ```js
    var s = "𠮷"; //该汉字码点为0x20BB7，UTF-16 编码为0xD842 0xDFB7
    
    s.length // 2
    s.charAt(0) // ''
    s.charAt(1) // ''
    s.charCodeAt(0) // 55362
    s.charCodeAt(1) // 57271
    ```
  
    能正确处理占位两个字节的字符，也是测试一个字符由两个字节还是由四个字节组成的最简单方法。
  
    ```js
    let s = '𠮷a';
    
    s.codePointAt(0) // 134071	𠮷的十进制码点
    s.codePointAt(1) // 57271	𠮷的后两个字节的十进制码点
    s.codePointAt(2) // 97
    
    //解决a的序号是2的问题
    let s = '𠮷a';
    for (let ch of s) {
      console.log(ch.codePointAt(0).toString(16));
    }
    // 20bb7
    // 61
    
    //判断一个字符是否为两个字节
    function is32Bit(c) {
      return c.codePointAt(0) > 0xFFFF;
    }
    is32Bit("𠮷") // true
    is32Bit("a") // false
    ```
  
  + **normalize()**
  
    将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化。
  
  + **includes(), startsWith(), endsWith()**
  
    传统上，JavaScript 只有`indexOf`方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法:
  
    + includes()	返回布尔值，表示是否找到了参数字符串
    + startsWith()	返回布尔值，表示参数字符串是否在原字符串的头部
    + endsWith()	返回布尔值，表示参数字符串是否在原字符串的尾部
  
    这三个方法都支持第二个参数，**includes()**和**startsWith()**表示开始搜索的位置，不填则默认重头开始搜索;**endsWith()**表示针对前`n`个字符。
  
    ```js
    let s = 'Hello world!';
    s.startsWith('world', 6) // true
    s.endsWith('Hello', 5) // true
    s.includes('Hello', 6) // false
    ```
  
  + **repeat()**
  
    接收一个数字作为参数，返回一个新字符串，表示将原字符串重复`n`次。
  
    ```js
    'x'.repeat(3) // "xxx"
    'hello'.repeat(2) // "hellohello"
    'na'.repeat(0) // ""
    ```
  
    若传入的参数为Infinity或者负数则会报错
  
    ```js
    'na'.repeat(Infinity)
    // RangeError
    'na'.repeat(-1)
    // RangeError
    ```
  
    若传入的参数为一个小数，则会取整再操作。
  
    ```js
    'na'.repeat(2.9) // "nana"
    
    //若参数为0到-1之间的数，则会转为0
    'na'.repeat(-0.9) // ""
    
    //NaN等同于0
    'na'.repeat(NaN) // ""
    
    //如果`repeat`的参数是字符串，则会先转换成数字
    'na'.repeat('na') // ""
    'na'.repeat('3') // "nanana"
    ```
  
  + **padStart()，padEnd()**
  
    字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全。
  
    `padStart()`和`padEnd()`一共接受两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串
  
    ```js
    'x'.padStart(5, 'ab') // 'ababx'
    'x'.padStart(4, 'ab') // 'abax'
    
    'x'.padEnd(5, 'ab') // 'xabab'
    'x'.padEnd(4, 'ab') // 'xaba'
    ```
  
    如果原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串。
  
    如果用来补全的字符串与原字符串，两者的长度之和超过了最大长度，则会截去超出位数的补全字符串。
  
    如果省略第二个参数，默认使用空格补全长度。
  
  + **trimStart()，trimEnd()**
  
    行为与`trim()`一致，`trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格。
  
    返回新的字符串，不会修改原始字符串。
  
  + **matchAll()**
  
    返回一个正则表达式在当前字符串的所有匹配

