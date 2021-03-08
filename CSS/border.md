1. **border**
   + `border-style, border-color, border-size`的简写形式
2. **border-image**
   + `border-image-source, border-image-slice, border-image-width, border-image-outset, border-image-repeat`的简写形式，其中`slice, width, outset`要遵循写法`slice/width/outset`
   + `border-image-source:`
     + 定义边框背影的来源
   + `border-image-slice:`
     + 定义图片切片的大小
     + `[number]:`偏移量
     + `[percentage]:`
   + `border-image-width:`
     + 定义边框背景的宽度，若宽度大于原始边框宽度，则向`padding-box`区域扩展
     + `[number]:`相当于原始边框宽度的倍数
     + `[percentage]:`占`border-box`大小的百分比
   + `border-image-outset:`定义边框可超出边框盒的大小
     + `[number]:`边框宽度的数量
     + `[length]:`固定值
   + `border-image-repeat:`
     + 定义边框背景的平铺方式
     + `repeat:`在不缩放图像的条件下重复图像
     + `round:`在可能缩放图像的条件下平铺图像
     + `space:`在不缩放图像的条件下平铺图像，不能整数次平铺时添加空白
     + `stretch:`拉伸图像以填充边框
3. **border-style**
   + `none:`无
   + `hidden:`隐藏
   + `dotted:`圆点
   + `dashed:`虚线
   + `solid:`实线
   + `double:`双线
   + `groove:`渐变线(?)
   + `ridge:`与`groove`效果相反
   + `inset:`凹陷效果
   + `outset:`凸显效果
4. **box-shadow**
   + `[x偏移] [y偏移] [模糊半径] [延伸半径] [颜色]`
   + `inset:`可选，阴影出现在边框外
   + `outset:`可选，阴影出现在边框内

