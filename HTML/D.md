**<data>**

将一个指定内容和机器可读的翻译联系在一起

```html
<ul>
    <li><data value='001'>一</data></li>
    <li><data value='002'>二</data></li>
    <li><data value='003'>三</data></li>
</ul>
```

**<datalist>**

包含一组options选项，表示其他表单控件的可选值

```html
<lable for='choice'>
	<input list='choice-list' id='choice'>
    <datalist id='choice-list'>
    	<options>One</options>
        <options>Two</options>
        <options>Three</options>
        <options>Four</options>
        <options>Five</options>
    </datalist>
</lable>
```

**<dl>, <dt>, <dd>**

用于术语的定义与描述

```html
<dl>
    <dt>Firefox</dt>
    <dd>A free, open source, cross-platform, graphical web browser developed by the Mozilla Corporation and hundreds of volunteers.</dd>
</dl>
```

**<del>**

用于表示被删除、修改的内容

```html
<del>这是一些被划掉的文本</del>
```

**<details>**

显示一些简略信息，当被展开时才会显示其中的全部内容，同<summary>一起使用

```html
<details>
	<summary>Click for more information</summary>
    This is details.
</details>
```

**<dfn>**

用于定义术语

```html
<p>
    <dfn title='dfn-Internet'>The Internet</dfn> is a global system of interconnected networks that use the Internet Protocol Suite (TCP/IP) to serve billions of users worldwide.
    
    <dl>
        <dt><dfn><abbr title='World-wide web'>WWW</abbr></dfn></dt>
        <dd>The World-Wide Web (WWW) is a system of interlinked hypertext documents accessed on <a href="#def-internet">the Internet</a>.</dd>
	</dl>
</p>
```

**<dialog>**

表示一个对话框或其他交互式组件

```html
<dialog open>
    <!-- 当设置了 open 属性时才会显示给用户 -->
	<p>
        Greeting!!
    </p>
</dialog>
```

**<div>**

