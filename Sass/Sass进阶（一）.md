---
title: Sass进阶（一）
date:
tags:
	- CSS
	- Scss
categories:
	- CSS
---

# Sass进阶

## @import与partials文件

+ `partial`文件以`_`开头，Sass会将其视为一个部件，而主文件没有下划线开头

+ `@import`用于引入外部的scss文件
+ `@import`引入外部partial文件时不用带上下划线

```scss
// _common.scss
* {
    padding: 0;
    margin: 0;
}


// index.scss
@import 'common';
```

## Sass注释

+ 多行注释：以`/*`开头，`*/`结尾
+ 单行注释：以`//`开头
+ 强制注释：前两个不会出现在压缩后的css文件中，而使用强制注释则会在压缩后的css文件中显示，强制注释在开头的字符后需紧跟一个`!`

## Sass函数

### 数字函数

+ `abs(number)`：绝对值
+ `round(number)`：四舍五入
+ `ceil(number)`：向上取整
+ `floor(number)`：向下取整
+ `percentage(number)`：转为百分比值
+ `min(number, number[, number...])`：求最小值
+ `max(number, number[, number...])`：求最大值

### 字符串函数

+ `to-upper-case(str)`：将字符串转为大写
+ `to-lower-case(str)`：将字符串转为小写
+ `str-length(str)`：计算字符串的长度
+ `str-index(str, char|str)`：计算字符或字符串在原字符串最开始出现的位置
+ `str-insert(str1, str2, position)`：将str2插入到str1的指定位置

### 颜色函数

+ `rgb(red, green, blue)`
+ `rgba(red, green, blue, alpha)`
+ `hsl(色相[0,360], 饱和度[0,100%], 明度[0,100%])`
+ `hsla(色相[0,360], 饱和度[0,100%], 明度[0,100%], alpha)`
+ `adjust-hue(color, hue)`：调整颜色的色相
+ `lighten(color, percentage)`：将颜色按百分比增量
+ `darken(color, percentage)`：将颜色按百分比调暗
+ `saturate(color, 饱和度)`：增加颜色的饱和度
+ `desaturate(color, 饱和度)`：降低颜色的饱和度
+ `transparentize(color, alpha[0,1])`：将颜色透明度按所给值调低
+ `opacify(color, alpha[0,1])`：将颜色透明度按所给值调高

### 列表函数

+ `length(list)`：计算列表的长度
+ `nth(list, index)`：寻找给定索引的项
+ `index(list, item)`：取得所给项的索引
+ `append(list, item[, 分隔符])`：向列表中添加项
+ `join(list, list[, 分隔符])`：合并列表

#### 键值对列表map

如`$map: (key1: value1, key2: value2, key3: value3)`

+ `map-get(map, item)`：获取所给项目的值
+ `map-keys(map)`：获取给定map的所有键名
+ `map-values(map)`：获取所给map的所有值
+ `map-has-key(map, key)`：判断所给map是否有某个项
+ `map-merge(map, map)`：合并map
+ `map-remove(map, item[,item...])`：删除map中的所给项

