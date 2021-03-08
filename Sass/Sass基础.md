---
title: Sass-基础
date:
tags:
	- CSS
	- Scss
categories:
	- CSS
---

# Sass-基础

## Sass变量

+ 使用`$`符号可以定义或使用一个变量
+ 变量中的`-`和`_`意义相同，即`$primary-color`和`$primary_color`被看作是同一个变量

```scss
//使用 $ 符号定义或使用变量
//定义
$color: red;
$border: 1px solid $color;

//使用
#app {
    background-color: $color;
    border: $border;
}
```

## Sass嵌套写法

+ 在嵌套的子选择器中可以使用`&`符号来引用父选择器

```scss
#app {
    height: 100px;
    div {
        height: 50%;
    }
}

#app {
    height: 100px;
    a {
        color: red;
        font-size: 20px;
        &:hover {
            color: blue;
        }
    }
    
    .nav {
        &-text {
            //被转为 .nav-text
            font-size: 10px;
        }
    }
}
```

### 属性嵌套

+ 若有一系列属性的前缀相同，那么可以将前缀提取出来，后加一对花括号`{}`，在其中以后缀为键名书写各个属性

``` scss
#app {
    font: {
        family: 'xxx';
        size: 20px;
        weight: bold;
    }
    
    border: 1px solid red {
        left: 0;
        right: 0;
        }
}
```

## mixin

+ 使用`@mixin`来定义一个mixin，使用`@include`引入一个mixin
+ `mixin`可以使用参数

```scss
@mixin alert {
    color: #8a6d3b;
    background-color: #fcf8e3;
}
.alert-warning {
    @include alert;
}

//参数
@mixin alert($color, $background){
    color: $color;
    ackground-color: $background;
}
.alert-warning {
    @include alert(#8a6d3b, #fcf8e3)
    //或
    @include alert($background: #fcf8e3, $color: #8a6d3b)
}
```

## 继承/扩展

+ 使用`@extend`继承一个与指定选择器相关的所有属性
+ 如果被继承的选择器定义有子选择器的样式，`@extend`同样会将其所有子选择器的样式继承过来

```scss
.alert {
    color: red;
}
.alert a {
    font-weight: bold;
}

.alert-info {
    @extend .alert;
    font-size: 20px;
}
```

