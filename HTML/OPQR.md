**<object>**

引入一个资源，可能是一张图片，一个嵌入的浏览上下文，或是一个插件所需要的资源

**<ol>**

有序列表

```html
<ol>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
</ol>
```

**<optgroup>**

为<select>元素中的选项分组

```html
<select>
    <optgroup label='one'>
    	<option value='1'>1</option>
    </optgroup>
    <optgroup label='two'>
    	<option value='2'>2</option>
        <option value='3'>3</option>
    </optgroup>
    <optgroup label='three'>
    	<option value='4'>4</option>
        <option value='5'>5</option>
    </optgroup>
</select>
```

**<option>**

\<select>, <optgroup>, <datalist>元素内定义的选项

**<output>**

表示用户操作或计算的结果

```html
<form oninput='three.value=parseInt(one.value)+parseInt(two.value)'>
    <input type='text' name='one'>
    <input type='text' name='two'>
    <output name='three'></output>
</form>
```

**<o>**

段落元素

**<param>**

在<object>元素内定义参数

**<picture>**

通过包含零个或多个<source>元素和一个<img>元素来为不同显示场景提供图像支持

```html
<picture>
    <source srcset='img2.jpg' media='(min-width: 700px)' type='image/jpg'>
    <img src='img1.jpg' alt='img1'>
</picture>
```

**<pre>**

表示预定义文本，其中的文本格式与原文件格式相同

**<progress>**

进度条

```html
<progress max='100' value='50'>50%</progress>
```

**<q>**

表示短的文本的引用（长文本引用可用<blockquote>

**<ruby>, <rp>, <rt>, <rtc>**

用来展示东亚文字注音或字符注释

<rp>为不支持<ruby>元素的浏览器显示其中的内容

<rt>为文字注音

<rtc>为文字添加语义注解

```html
<ruby>
    汉<rp>(</rp><rt>han</rt><rp>)</rp>
    字<rp>(</rp><rt>zi</rt><rp>)</rp>
</ruby>
```