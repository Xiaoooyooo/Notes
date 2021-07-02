# vertical-align

在css中`vertical-align`主要是用来指定行内元素与表格单元格的垂直对齐方式。

它常用于两种情况：

+ 使行内元素盒模型与其行内元素容器垂直对齐。例如，用于垂直对齐一行文本的内的图片；
+ 垂直对齐表格单元内容。

在开始之前有必要知道行内元素的几条线，如下图所示：

![线](https://upload.wikimedia.org/wikipedia/commons/3/39/Typography_Line_Terms.svg)

其中最重要的就是`median(中线)`和`baseline(基线)`，值得注意的是这两条线之间的距离正好是一个字母`x`的高度，即`x-height`。大概知道了几条线的位置，接下来开始介绍`vertical-align`的各个属性。

## vertical-align的值

### baseline

等同于行内元素的默认效果，即该元素与其父元素容器的基线位置对齐。

### sub

使元素的基线与其父元素下表文本的基线位置对齐。

### super

使元素的基线与其父元素的上标文本基线对齐。

### text-top

使元素顶部与其父元素文本顶部对齐。

### text-bottom

使元素底部与其父元素文本底部对齐。

### middle

使元素中部与其父元素的基线加上`x-height`的一半对齐。

### [length]、[percentage]

使元素的基线对齐到父元素基线之上的给定高度，可以是负数。

### top

以元素内最高的子元素为基准与其父元素的文本顶部对齐

### bottom

以元素内最低的子元素为基准与其父元素的文本底部对齐

## 实际使用

### 图标居中对齐

### 表格内容对齐

## 参考资料

+ [vertical-align MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)
+ [字母’x’在CSS世界中的角色和故事](https://www.zhangxinxu.com/wordpress/2015/06/about-letter-x-of-css/)

