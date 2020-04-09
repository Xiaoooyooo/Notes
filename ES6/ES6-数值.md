# ES6-数值

## 二进制和八进制表示法

ES6 提供了二进制和八进制数值的新的写法，分别用前缀`0b`（或`0B`）和`0o`（或`0O`）表示。

```js
0b111110111 === 503 // true
0o767 === 503 // true
```

如果要将`0b`和`0o`前缀的字符串数值转为十进制，要使用`Number`方法。

```js
Number('0b111')  // 7
Number('0o10')  // 8
```

## Number.isFinite(), Number.isNaN()

`Number.isFinite()`用来检查一个数值是否为有限的（finite），即不是`Infinity`。对于非数值一律返回`false`。

`Number.isNaN()`用来检查一个值是否为`NaN`。如果参数类型不是`NaN`，一律返回`false`。

## Number.parseInt(), Number.parseFloat()

ES6 将全局方法`parseInt()`和`parseFloat()`，移植到`Number`对象上面，行为完全保持不变。

```js
// ES5的写法
parseInt('12.34') // 12
parseFloat('123.45#') // 123.45

// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45

Number.parseInt === parseInt // true
Number.parseFloat === parseFloat // true
```

这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

## Number.isInteger()

判断一个数值是否为整数，如果参数不是数值，`Number.isInteger`返回`false`。

```js
Number.isInteger() // false
Number.isInteger(null) // false
Number.isInteger('15') // false
Number.isInteger(true) // false
```

由于 JavaScript 采用 IEEE 754 标准，数值存储为64位双精度格式，数值精度最多可以达到 53 个二进制位（1 个隐藏位与 52 个有效位）。如果数值的精度超过这个限度，第54位及后面的位就会被丢弃，这种情况下，`Number.isInteger`可能会误判。

如果一个数值的绝对值小于`Number.MIN_VALUE`（5E-324），即小于 JavaScript 能够分辨的最小值，会被自动转为 0。这时，`Number.isInteger`也会误判。

## Number.EPSILON

ES6 在`Number`对象上面，新增一个极小的常量`Number.EPSILON`。根据规格，它表示 1 与大于 1 的最小浮点数之间的差。
`Number.EPSILON`实际上是 JavaScript 能够表示的最小精度。误差如果小于这个值，就可以认为已经没有意义了，即不存在误差了。
`Number.EPSILON`可以用来设置“能够接受的误差范围”。比如，误差范围设为 2 的-50 次方（即`Number.EPSILON * Math.pow(2, 2)`），即如果两个浮点数的差小于这个值，我们就认为这两个浮点数相等。

```js
//为浮点数运算，部署了一个误差检查函数
function withinErrorMargin (left, right) {
  return Math.abs(left - right) < Number.EPSILON * Math.pow(2, 2);
}

0.1 + 0.2 === 0.3 // false
withinErrorMargin(0.1 + 0.2, 0.3) // true

1.1 + 1.3 === 2.4 // false
withinErrorMargin(1.1 + 1.3, 2.4) // true
```

## 安全整数和 Number.isSafeInteger()

JavaScript 能够准确表示的整数范围在`-2^53`到`2^53`之间（不含两个端点），超过这个范围，无法精确表示这个值。
ES6 引入了`Number.MAX_SAFE_INTEGER`和`Number.MIN_SAFE_INTEGER`这两个常量，用来表示这个范围的上下限。
`Number.isSafeInteger()`则是用来判断一个整数是否落在这个范围之内。

```js
Number.MAX_SAFE_INTEGER === Math.pow(2, 53) - 1
// true
Number.MAX_SAFE_INTEGER === 9007199254740991
// true

Number.MIN_SAFE_INTEGER === -Number.MAX_SAFE_INTEGER
// true
Number.MIN_SAFE_INTEGER === -9007199254740991
// true
```

如果只验证运算结果是否为安全整数，很可能得到错误结果，应该同时验证两个运算数和运算结果。

```js
function trusty (left, right, result) {
  if (
    Number.isSafeInteger(left) &&
    Number.isSafeInteger(right) &&
    Number.isSafeInteger(result)
  ) {
    return result;
  }
  throw new RangeError('Operation cannot be trusted!');
}

trusty(9007199254740993, 990, 9007199254740993 - 990)
// RangeError: Operation cannot be trusted!

trusty(1, 2, 3)
// 3
```

## Math 对象的扩展

+ **Math.trunc()**

  去除一个数的小数部分，返回整数部分，对于非数值，`Math.trunc`内部使用`Number`方法将其先转为数值，对于空值和无法截取整数的值，返回`NaN`。
  
  ```js
  Math.trunc(4.1) // 4
  Math.trunc(4.9) // 4
  Math.trunc(-4.1) // -4
  Math.trunc(-4.9) // -4
  Math.trunc(-0.1234) // -0
  
  Math.trunc('123.456') // 123
  Math.trunc(true) //1
  Math.trunc(false) // 0
  Math.trunc(null) // 0
  
  Math.trunc(NaN);      // NaN
  Math.trunc('foo');    // NaN
  Math.trunc();         // NaN
  Math.trunc(undefined) // NaN
  ```
  
+ **Math.sign()**

  用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。对于那些无法转为数值的值，会返回`NaN`。
  它会返回五种值：

  - 参数为正数，返回`+1`；
  - 参数为负数，返回`-1`；
  - 参数为 0，返回`0`；
  - 参数为-0，返回`-0`;
  - 其他值，返回`NaN`。

+ **Math.cbrt()**

  计算一个数的立方根。对于非数值，`Math.cbrt`方法内部也是先使用`Number`方法将其转为数值。

+ **Math.clz32()**

  计算一个数的 32 位二进制形式的前导 0 的个数。

  ```js
  Math.clz32(0) // 32
  Math.clz32(1) // 31
  Math.clz32(1000) // 22
  Math.clz32(0b01000000000000000000000000000000) // 1
  Math.clz32(0b00100000000000000000000000000000) // 2
  ```

  对于小数，`Math.clz32`方法只考虑整数部分。

+ **Math.imul()**

  返回两个数以 32 位带符号整数形式相乘的结果，返回的也是一个 32 位的带符号整数。

+ **Math.fround()**

  返回一个数的32位单精度浮点数形式。

+ **Math.hypot()**

  返回所有参数的平方和的平方根。

  ```js
  Math.hypot(3, 4);        // 5
  Math.hypot(3, 4, 5);     // 7.0710678118654755
  Math.hypot();            // 0
  Math.hypot(NaN);         // NaN
  Math.hypot(3, 4, 'foo'); // NaN
  Math.hypot(3, 4, '5');   // 7.0710678118654755
  Math.hypot(-3);          // 3
  ```

+ **对数方法**

  + **Math.expm1()**

    返回 ex - 1，即`Math.exp(x) - 1`。

  + **Math.log1p()**

    返回`1 + x`的自然对数，即`Math.log(1 + x)`。如果`x`小于-1，返回`NaN`。

  + **Math.log10()**

    返回以 10 为底的`x`的对数。如果`x`小于 0，则返回 NaN。

  + **Math.log2()**

    返回以 2 为底的`x`的对数。如果`x`小于 0，则返回 NaN。

+ **双曲函数方法**

  ES6 新增了 6 个双曲函数方法：

  - `Math.sinh(x)` 返回`x`的双曲正弦（hyperbolic sine）
  - `Math.cosh(x)` 返回`x`的双曲余弦（hyperbolic cosine）
  - `Math.tanh(x)` 返回`x`的双曲正切（hyperbolic tangent）
  - `Math.asinh(x)` 返回`x`的反双曲正弦（inverse hyperbolic sine）
  - `Math.acosh(x)` 返回`x`的反双曲余弦（inverse hyperbolic cosine）
  - `Math.atanh(x)` 返回`x`的反双曲正切（inverse hyperbolic tangent）

##  指数运算符 **

```js
2 ** 2 // 4
2 ** 3 // 8

2 ** 3 ** 2		// 相当于 2 ** (3 ** 2)
// 512

let a = 1.5;
a **= 2;
// 等同于 a = a * a;
let b = 4;
b **= 3;
// 等同于 b = b * b * b;
```

