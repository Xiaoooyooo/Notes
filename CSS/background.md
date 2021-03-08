1. **background**
   + `background-iamge, background-repeat, background-attachment, background-position, background-size, background-origin, background-clip, background-color`的简写形式，其中`background-size`一定要写在`background-position`后面并用`/`隔开
   + 其中`background-origin, background-clip`较为特殊，这两个属性的可选值一样，如果只指定了一个值，那么该值会被同时应用到两个属性上，如果指定了两个值，则前一个值用于设置`background-origin`，后一个值用于设置`background-clip`
   + 可同时设置多组`background`的值，每个值用逗号隔开

2. **background-attachment**
   + `fixed:`背景相对于视口固定
   + `scroll:`不相对于视口固定，且不随元素内容滚动
   + `local:`不相对于视口固定，随元素内容滚动
3. **background-clip**
   + `border-box:`元素背景可延伸到`border`区域
   + `padding-box:`元素背景可延伸到`padding`区域
   + `content-box:`元素背景仅在内容区域
   + `text:`元素背景被裁为文字的前景色
4. **background-image**
   + `url()`
   + `linear-gradient()`
   + `radial-gradient()`
5. **background-origin**
   + `border-box:`背景原点在`border`上
   + `padding-box:`背景原点在`padding`上
   + `content-box:`背景原点在`content`上