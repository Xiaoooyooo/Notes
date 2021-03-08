**<em>**

标记出需要用户着重阅读的内容

**<embed>**

将外部内容嵌入文档中的指定位置，该内容通常由外部应用程序提供

**<fieldset>**

对表单中的元素进行分组

```html
<form>
    <fieldset>
        <legend>This is title</legend>
        <input type='radio' name='one' id='one'>
        <label for='one'>One</label>
        <input type='radio' name='one' id='two'>
        <label for='two'>two</label>
        <input type='radio' name='one' id='three'>
        <label for='three'>three</label>
    </fieldset>
</form>
```

**<figure>**

表示一段独立的内容，常与**<figcaption>**一起使用，作为主文中引用的图片，插图，表格，代码段等。

```html
<figure>
	<img src='xxxxxx' alt='xxxxx' />
    <figcaption>This is the caption of the picture</figcaption>
</figure>
```

**<footer>**

表示最近一个章节内容或者根节点元素的页脚

```html
<acticle>
	<!-- .... -->
    <footer><!-- .... --></footer>
</acticle>


<body>
    <!-- .... -->
    <footer><!-- .... --></footer>
</body>
```

**<form>**

表单元素