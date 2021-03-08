**<label>**

用户界面某个元素的说明，常与<input>一起使用

```html
<label>
	Name <input type='text' />
</label>
<!-- 同 -->
<label for='name'>Name</label>
<input id='name' />
```

**<legend>**

用于表示其父元素<fieldset>的内容标题

```html
<form>
    <fieldset>
        <legend>Title</legend>
        <label for='name'>Name:</label>
        <input type='text' />
    </fieldset>
</form>
```

**<li>**

用于表示列表的条目，必须包含在一个父元素中，如<ol>,<ul>,<menu>

**<link>**

规定当前页面与外部资源的关系，常用于链接样式表和创建站点图标favicon

**<main>**

呈现<body>元素内的主体部分

**<map>**

与<area>一起定义图片的映射（一个可点击的链接区域）

```html
<img src='xxxx' usemap='#mm' alt='image'>
<map name='mm'>
	<area shape='circle' coords='200,200,100' href='xxxxx'>
    <!-- 其他映射 -->
</map>
```

**<mark>**

用于高亮文本

**<menu>**

呈现一组用户可执行的操作或命令

**<menuitem>**

<menu\>子元素

**<meta>**

[mdn <meta>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta)

表示不能由其他html元相关元素（base. link, script, style, title）表示的元数据。

`meta`元素定义的元数据类型包括：

+ `name:`提供文档级别的元数据，应用于整个页面
  + `description:`网页的描述
  + `keywords:`网页关键词
+ `http-equiv:`编译指令，提供的信息类似于命名的HTTP头部
  + `content-type:`值必须为`text/html; charset=utf-8`
  + `refresh:` `[数值]`或`[数值]；url=xxxxx`
  + `x-ua-compatiple:`值必须为`IE=edge`
+ `charset:`告诉文档应当使用哪种字符集
+ `itemprop:`提供用户定义的元数据

**<meter>**

用于显示已知范围的标量值或分量值

```html
<meter min='200' max='500' value='350'></meter>
```

**<nav>**

在当前文档或其他文档中提供导航链接

**<noscript>**

如果页面上的脚本类型不受支持或者当前在浏览器中关闭了脚本，则在该元素中定义脚本未被执行时的替代内容。