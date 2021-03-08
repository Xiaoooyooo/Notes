---
title: Sass进阶（二）
date: 
tags:
	- CSS
	- Scss
categories:
	- CSS
---

# Sass进阶（二）

## interpolation

格式`#{变量名}`使用这种方式能在任何地方使用已经定义的scss变量

```scss
$text: 'hello world';
//在注释中使用 #{$text}

$name: 'info';
$attr: 'border';
.alert-#{$name} {
    #{$attr}-color: red;
}
```

## @if, @else if, @else

判断语句，当条件为真时才使用其中的css代码

```scss
$theme: 'dark';

body {
    @if $theme == dark {
        background-color: black;
    } @else if $theme == light {
        background-color: white
    } @else {
      background-color: grey;  
    }
}
```

## @for

循环语句

> @for $变量 from 开始值 through/to 结束值 {
> 	每次循环需要做的事
> 	}

```scss
$columns = 4;
@for $i from 1 through $columns {
    .col-#{$i} {
        width: 100% / $columns * $i;
    }
}
/*
 * through和to的区别
 * through：当$i等于$columns时还会进行一次处理
 * to：当$i等于$columns时即结束循环
*/
```

## @each

遍历列表，语法：`@each $变量名 in $列表{}`

```scss
$icons: success error warning;

@each $icon in $icons {
    .icon-#{$icon} {
        background-image: url('./image/#{$icon}.png')
    }
}
```

## @while

语法：`@while 条件 {}`

```scss
$i: 6;
@while $i > 0 {
    .item-#{$i}{
        width: 10px * $i;
    }
    $i: $i - 2;
}
```

## 自定义函数@function

语法：`@function 函数名 ([参数]){}`，使用`@return`返回需要的值

```scss
$colors: (light: white, dark: black);
@function color($key){
    @return map-get($colors, $key)
}

body {
    background-color: color(light);
}
```

## 警告@warn与错误@error

```scss
//警告 输出在控制台
@warn "xxxxxxxxxx!"
    
//错误 输出在编译结果
@error "XXXXXXXXX!!"
```

