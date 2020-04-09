# JavaScrit面向对象 OOP

## 术语

+ **Namespace 命名空间**
  允许开发人员在一个独特，应用相关的名字的名称下捆绑所有功能的容器。

+ **Class 类**
  定义对象的特征。它是对象的属性和方法的模板定义。

+ **Object 对象**
  类的一个实例。

+ **Property 属性**
  对象的特征，比如颜色。

+ **Method 方法**
  对象的能力，比如行走。

+ **Constructor 构造函数**
  对象初始化的瞬间，被调用的方法。通常它的名字与包含它的类一致。

+ **Inheritance 继承**
  一个类可以继承另一个类的特征。

+ **Encapsulation 封装**
  一种把数据和相关的方法绑定在一起使用的方法。

+ **Abstraction 抽象**
  结合复杂的继承，方法，属性的对象能够模拟现实的模型。

+ **Polymorphism 多态**
  多意为「许多」，态意为「形态」。不同类可以定义相同的方法或属性。

  

  [更多关于面向对象编程的描述](http://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1%E2%80%8B)

## Javascript面向对象编程

#### 命名空间

**注：在JavaScript中，命名空间只是另一个包含方法，属性，对象的对象**

```js
//创建一个全局命名空间
var myapp = myapp || {}

//创建子命名空间
myapp.child = {}

// 给普通方法和属性创建一个叫做MYAPP.commonMethod的容器
myapp.commonMethod = {
  regExForName: "", // 定义名字的正则验证
  regExForPhone: "", // 定义电话的正则验证
  validateName: function(name){
    // 对名字name做些操作，你可以通过使用“this.regExForname”
    // 访问regExForName变量
  },
 
  validatePhoneNo: function(phoneNo){
    // 对电话号码做操作
  }
}

// 对象和方法一起申明
MYAPP.event = {
    addListener: function(el, type, fn) {
    //  代码
    },
   removeListener: function(el, type, fn) {
    // 代码
   },
   getEvent: function(e) {
   // 代码
   }
  
   // 还可以添加其他的属性和方法
}

//使用addListener方法的写法:
MYAPP.event.addListener("yourel", "type", callback);
```

---

#### 继承

**Javascript只支持单继承**

