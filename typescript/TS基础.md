---
title: TypeScript基础
date: 2021-03-13 15:13:24
tags:
	- TypeScript
	- JavaScript
categories:
	- TypeScript
---

# TypeScript基础

## 变量申明

**ts数据类型**：

+ number
+ string
+ boolean
+ 字面量（其本身）
+ any：可以被赋予任意值，能赋值给任意变量
+ unkonwn：可以被赋予任意值，不能赋值给任意变量
+ void：表示函数返回值为空（没有返回值）
+ never：表示函数永远不会返回结果（例如函数在运行中报错）
+ object
+ array
+ tuple：元组，固定长度的数组
+ enum：枚举，将同一类型的值的可能出现的值整合到一起

```ts
/* 变量声明格式
 * var/let/const [propName]:[type]
 * 变量名与变量类型之间用冒号连接
 */

//number
let num: number;
num = 10;

//string
let str: string;
str = 'hello world'

//boolean
let bool: boolean;
bool = false;

//字面量
let a: 'aaa' | 'bbb'
a = 'aaa'
a = 'ccc' //出错

//any
let an: any;
an = 'aa'
an = 12
a = an //允许

//unknown
let unkn: unknown;
unkn = 12
unkn = 'aaa'
a = unkn //出错

//void
let voidfn():void{}

//never
let neverfn():never{
    throw 'error'
}

//object
let obj: {
    name: string,
    age: number,
    [propName: string]: any
}
obj = {
    name: 'tom',
    age: 20,
    gender: 'male'
}

//array 不限制长度的数组
let arr: number[] //或 arr: Array<number>
arr = [1,2,3,4]

//tuple 限制长度的数组
let tuplearr: [number, string]
tuplearr = [12, 'aaa']

//enum
enum en;
en = {male,famale,unknown}
```

**与js的不同**

+ ts中如果没有指定则一个变量一般只能用于存储一种数据类型
+ js中变量永远是any类型，而ts中仅申明而未赋值或申明为any类型的变量才为any类型



