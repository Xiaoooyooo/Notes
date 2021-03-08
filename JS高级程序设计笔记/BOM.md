# BOM

## window

### window、top、parent、self

+ 在没有框架（iframe）的条件下，top一定等于parent，且二者等于window；
+ 在有框架的条件下，top可能等于parent，top指向最外层window，parent指向父级框架的window；
+ self始终指向window

### window.moveTo(), window.moveBy()

+ 这两个方法都是用于移动窗口位置的
+ 都接受两个参数x, y，`moveTo()`用于移动到指定位置，`moveBy()`用于移动指定的偏移量
+ 仅对最外层window有效
+ 可能被浏览器禁用

### window.innerHeight, window.innerWidth, window.outerHeight, window.outerWidth

+ 用于确认浏览器的窗口区域的大小
+ `inner`前缀表示页面视图区的大小，`outer`前缀表示页面视图容器的大小（包括浏览器边框）

### document.documentElement.clientWidth, document.documentElement.clientHeight, document.body.clientWidth, document.body.clientHeight

+ 用于保存页面视口信息
+ 前两个在标准模式下有效，后两个在混杂模式下有效

### window.resizeTo(), window.resizeBy()

+ 都用于修改窗口大小，接受两个参数
+ 前者修改到固定尺寸，后者按给定参数进行扩大或减小
+ 可能被浏览器禁用

### window.open()

+ 导航到一个特定的url，或打开一个新的窗口

+ 接受四个参数：url，窗口目标（name），用逗号相隔的特性字符串，表示新页面是否取代浏览器历史记录当前页的布尔值，关于第三个参数的说明如下

  |    属性    |   值   |           说明            |
  | :--------: | :----: | :-----------------------: |
  | fullscreen | yes/no |         是否全屏          |
  |  location  | yes/no |      是否显示地址栏       |
  |  menubar   | yes/no |      是否显示菜单栏       |
  | resizable  | yes/no | 是否可以拖动边框改变大小  |
  | scrollbars | yes/no |       是否允许滚动        |
  |   status   | yes/no |      是否显示状态栏       |
  |  toolbar   | yes/no |      是否显示工具栏       |
  |    top     |  数值  | 新窗口的上坐标（不能为负) |
  |    left    |  数值  | 新窗口的左坐标（不能为负  |
  |   height   |  数值  |  新窗口的高度（不小于100  |
  |   width    |  数值  |  新窗口的宽度（不小于100  |

  

