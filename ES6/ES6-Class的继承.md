# ES6-Class的继承

Class 可以通过`extends`关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。

```js
class Father{
    constructor(){
        
    }
    foo(){
        return "foo"
    }
}
class Son extends Father{
    constructor(){
        super()
    }
    bar(){
        console.log(`bar-${super.foo()}`)
    }
}
let s = new Son()
s.bar()		//bar-foo
```

子类中出现的`super`关键字表示父类的构造函数，用来新建父类的`this`对象。**子类必须在`constructor`函数中调用`super`方法**，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。**在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字**，否则会报错。这是因为子类实例的构建，基于父类实例，只有`super`方法才能调用父类实例。

## Object.getPrototypeOf()

`Object.getPrototypeOf`方法可以用来从子类上获取父类。因此，可以使用这个方法判断，一个类是否继承了另一个类。

```js
class Father{}
class Son extends Father{}
Object.getPrototypeOf(Son) === Father	//true
```

## super 关键字

`super`这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。

+ 作为函数时

  `super`作为函数调用时，代表父类的构造函数，并且只能用在子类的`constructor`函数中。ES6 要求，子类的构造函数必须执行一次`super`函数。

  ```js
  class Father{
      constructor(){
          console.log(new.target.name)
      }
  }
  class Son extends Father{
      constructor(){
          super()	//相当于相Father.prototype.constructor.call(this)
      }
  }
  new Father	//Father
  new Son		//Son
  ```

+ `super`作为对象时

  + 在普通方法中，指向父类的原型对象；

    在子类普通方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类实例。

    由于`this`指向子类实例，所以如果通过`super`对某个属性赋值，这时`super`就是`this`，赋值的属性会变成子类实例的属性。

      ```js
      class Father{
          that(){
              return this
          }
          foo(){
              return "foo"
          }
      }
      class Son extends Father{
          constructor(){
              super()
              super.bar = 'bar'
              console.log(super.foo())
          }
          that(){
              console.log(this === super.that())
          }
      }
      let s = new Son()	//foo
      s.that()		//this === super.that()? true
      s.bar			//bar
      ```
  
  + 在静态方法中，指向父类。
  
    如果`super`作为对象，用在静态方法之中，这时`super`将指向父类，而不是父类的原型对象。
  
    ```js
    class Father{
        static foo(){
            return 'Static foo'
        }
        foo(){
            return 'foo'
        }
    }
    class Son extends Father{
        constructor(){
            super()
        }
        static bar(){
            console.log(super.foo())
        }
        bar(){
            console.log(super.foo())
        }
    }
    let s = new Son()
    s.bar()		//foo
    Son.bar()	//Static foo
    ```

## 原生构造函数的继承

原生构造函数是指语言内置的构造函数，通常用来生成数据结构。ECMAScript 的原生构造函数大致有下面这些：

- Boolean()
- Number()
- String()
- Array()
- Date()
- Function()
- RegExp()
- Error()
- Object()

ES6 允许继承原生构造函数定义子类，因为 ES6 是先新建父类的实例对象`this`，然后再用子类的构造函数修饰`this`，使得父类的所有行为都可以继承。