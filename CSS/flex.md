**重点：**

+ `align`控制**垂直于**容器主轴方向上项目的对齐方式
+ `justify`控制**平行于**容器主轴方向上项目的对齐方式
+ 在`Flexbox`中，`justify-items`与`justify-self`属性将失效，因为`justify-content`足以处理

1. **align-content**
   + 控制`Flexbox`中垂直于主轴（主轴默认为`row`方向）方向上的对齐方式
   + `normal:`默认对齐
   + 基本对齐关键字
     + `center:`
     + `start:`
     + `end:`
     + `flex-start:`
     + `flex-end:`
   + 基线对齐
     + `baseline:`
     + `first baseline:`
     + `last baseline:`
   + 分布式对齐
     + `space-between:`均匀分布项目，第一项与起始点齐平，最后一项与终止点齐平
     + `space-around:`均匀分布项目，项目在两端有一般大小的空间
     + `space-evenly:`均匀分布项目，项目在周围有相等空间
     + `stretch:`均匀分布项目，`height: auto`的项目会被拉伸
   + 溢出对齐
     + `safe [keyword]:`如果所给关键字的对齐方式会使项目溢出，则选用备用的对齐方式
     + `unsafe [keyword]:`不管项目会如何显示，都使用所给关键字的对齐方式
2. **align-items**
   + 定义`Flexbox`中每一行中的每个项目的对齐方式
   + `normal:`
   + `stretch:`
   + `center:`
   + `start:`
   + `end:`
   + `flex-start:`
   + `flex-end:`
   + `baseline:`
3. **align-self**
   + 定义`Flexbox`内项目的对齐方式，如果设定了该值，那么其父元素的`align-items`会失效
   + `auto:`
   + `normal:`
   + `center:`
   + `start:`
   + `end:`
   + `self-start:`
   + `self-end:`
   + `flex-start:`
   + `flex-end:`
   + `baseline:`
4. **column-gap**
   + 设置每列元素间的间隔
   + `normal:`多列布局时默认为`1em`，其他布局时默认为`0`
   + `[length]:`固定值
   + `[percentage]:`百分数
5. **row-gap**
   + 设置每行项目间的间隔
   + 参数同`column-gap`
6. **gap**
   + `row-gap`与`column-gap`的简写形式，若仅有一个值，则这个值同时设置为`row-gap`和`column-gap`；若有两个值，则前一个代表`row-gap`，后一个代表`column-gap`
   + `[length]:`固定值
   + `[percentage]:`相对容器的百分比
7. **justify-content**
   + 控制`Flexbox`中沿着容器主轴的项目的对齐方式
   + 属性值同`align-content`
8. **place-content**
   + `align-content`与`justify-content`的简写形式，若仅给定一个值，则该值同时设置为`align-content`与`justify-content`；若给定两个值，则前一个值表示`align-content`，后一个值表示`justify-content`
9. **place-items**
   + `align-items`与`justify-items`的简写形式，若仅给定一个值，则该值同时设置为`align-items`与`justify-items`；若给定两个值，则前一个值表示`align-items`，后一个值表示`justify-items`
10. **place-self**
    + `align-self`与`justify-self`的简写形式