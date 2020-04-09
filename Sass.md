# Sass

## Variables

使用`$`关键字定义一个变量，变量值为相应的属性值，任何值或值的一部分。

```scss
//定义
$color: red;
$border: 1px solid;

//使用
h1{
    color:$color;
    border: pink $border;
}
```

## Mixins

使用`@mixin`关键字定义一个混合，在其他地方使用`@include`关键字使用。

```scss
/* 定义一个mixin */
@mixin style($color,$font){
    color: $color;
    font-size: $font;
}

/* 使用 */
h1{
    text-align:center;
    @include style(orange,50px);
}
```

## Extend/Inheritance

使用`%`关键字定义一个扩展，在其他地方使用`@extend`关键词使用

```scss
//定义一个扩展
%hello{
    background: pink;
    border-radius: 20px;
}

//使用
h1{
    @extend %hello;
}
```

## Operators

sass支持在其中嵌入数学运算表达式。

```scss

```

