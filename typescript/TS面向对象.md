---
title: TS面向对象
date: 2021-03-13 20:33:14
tags:
	- TypeScript
	- JavaScript
categories:
	- TypeScript
---

# TS面向对象

## 类(class)

类似于JS，不同之处在于以下几点：

+ 属性
  + 在`constructor`中定义属性前需要在外部声明属性的数据类型
  + 在类的定义中存在静态属性的声明，使用`static`关键字声明，所谓静态属性即只能通过类名进行访问
  + 在类的实例中存在只读属性的声明，使用`readonly`关键字声明
    + `static`和`readonly`可以同时使用，此时`static`必须在前面，表示只读的静态属性
+ 方法
  + 实例方法：不需加任何关键字直接定义的方法
  + 静态方法：需要使用`static`关键字，定义的方法只能通过类名调用

```tsx
class Student{
    //实例属性
    name: string;
	//只读属性
	readonly gender: string;
	//静态属性
	static school:string = 'XXX School'
	constructor(name:string, gender: string){
        this.name = name
        this.gender = gender
    }
	//实例方法
	sayHi(){
        console.log(`Hi! Im'${this.name}.`)
    }
	//静态方法
	static sayHello(){
        cosole.log('Hello!')
    }
}
```

## 继承

当定义一个新的类时，如果发现其中需要定义的属性和方法与已有的一个类类似，则我们可以使用`extends`关键词来继承已有类中定义的属性和方法，此时有以下注意事项：

+ 子类可以继承父类中所有属性和方法（包括静态属性和静态方法）
+ 子类中存在一个特殊关键字`super`，用来表示父类
+ 如果子类中声明了构造函数，那么必须在子类的构造函数中调用`super()`，防止父类构造函数被覆盖

```tsx
// 定义动物类
class Animal {
    name: string;
    agr: number;
    constructor(name:string, age:number){
        this.name = name
        this.age = age
    }
    run(){
        console.log(`${this.name}在跑。`)
    }
}

// 定义猫类继承动物类
class Cat extends Animal {
    
}
```

## 抽象类

抽象类使用`abstract`关键词定义，使用这个关键词定义的类不能直接被用来作为构造函数创建实例，但可以被其他类继承

抽象类中可以定义抽象方法，具体的实现则交由子类实现

```tsx
abstract class Animal {
    name:string;
    constructor(name:string){
        this.name = name
    }
	//定义抽象方法，子类必须实现
	abstract say();
}

class Cat extends Animal {
    name: string;
	age: number;
    constructor(name,age){
        super(name)
        this.age = age
    }
	//子类必须实现抽象父类中定义的抽象方法
	say(){
        console.log("喵~")
    }
}
```

## 接口

使用`interface`关键词定义一个接口，它用来定义一个类或对象中应该包含哪些方法

定义类时可以使用`implements`实现一个接口

```tsx
interface myInterface {
    name: string;
    age: number;
}
class myClass implements myInterface {
    name: string;
	age: number;
}
```

**PS：`interface`与`type`**的不同

+ 定义接口时不需要加等号，使用`type`需要加等号
+ 不能使用`type`重新定义已定义的变量，`interface`表现为在原接口上扩展新规则

```tsx
type myType = {
    name: string;
    age: number;
}
type myType = {	// myType已存在，不能重新定义
    gender: string
}

interface myInterface {
    name: string;
    age: number
}
interface myInterface {	//myInterface已存在，扩展其中的规则
    gender: string
}
```

## 属性的封装（属性修饰符）

+ `public:`共有属性，可在任意位置访问
+ `pravite:`私有属性，只能在当前类内部访问
+ `protected:`只能在当前类和当前类的子类中访问

```tsx
class MyClass {
    public name: string;
	private gender: string;
	constructor(name:string, gender: string){
        this.name = name
        this._gender = gender
    }
	set gender(value: string){
        //可通过setter修改私有属性的值
        this._gender - value
    }
	get gender(){
        //可通过getter获取私有属性的值
        return this._gender
    }
}
```

定义一个类时可以使用以下方法：

```tsx
class A {
    constructor(public a: string, public b: number){
        this.a = a
        this.b = b
    }
}
```

