# html 冷门标签总结

**1.<abbr></abbr>** 标记一个缩写

```html
<p>The <abbr title="People's Republic of China">PRC</abbr> was founded in 1949.</p>
```

**2.<address></address>** 定义文档作者/所有者的联系信息
**提示：**不应该使用 <address> 标签来描述邮政地址，除非这些信息是联系信息的组成部分
**提示：**address 元素通常被包含在footer元素的其他信息中。

```html
<!--
如果 <address> 元素位于 <body> 元素内部，则它表示该文档作者/所有者的联系信息。
如果 <address> 元素位于 <article> 元素内部，则它表示该文章作者/所有者的联系信息。
-->
<address>
Written by <a href="mailto:webmaster@example.com">Jon Doe</a>.<br> 
Visit us at:<br>
Example.com<br>
Box 564, Disneyland<br>
USA
</address>
```

**3.<area/>** 定义图像映射内部的区域，且始终嵌套在map标签内

**提示：** img标签中的 usemap 属性与 map 标签中的 name 属性相关联，以创建图像与映射之间的关系

```html
<img src="planets.gif" width="145" height="126" alt="Planets" usemap="#planetmap">

<map name="planetmap">
  <area shape="rect" coords="0,0,82,126" alt="Sun" href="sun.htm">
  <area shape="circle" coords="90,58,3" alt="Mercury" href="mercur.htm">
  <area shape="circle" coords="124,58,8" alt="Venus" href="venus.htm">
</map>
```

**4.<article></article>** 定义独立的文章内容

```html
<article>
  <h1>Internet Explorer 9</h1>
  <p> Windows Internet Explorer 9(缩写为 IE9 )在2011年3月14日21:00 发布。</p>
</article>
```

**5.<aside></aside>** 表示一个和其余页面内容几乎无关的部分，通常表现为侧边栏或者标注框

**6.<base></base>** 为页面上的所有的**相对链接**规定默认(href) URL 或默认目标(target)

**提示：** 在一个文档中，最多能使用一个 base 标签。base 标签必须位于 head 元素内部。

```html
<base href="http://www.runoob.com/images/" target="_blank">

<!-- 此处img的连接为 http://www.runoob.com/images/logo.png -->
<img src="logo.png" width="24" height="39" alt="Stickman">
<!-- 此处 a 标签的 target 属性为 _blank -->
<a href="http://www.runoob.com">runoob.com</a>
```

**7.<bdi></bdi>** 允许您设置一段文本，使其脱离其父元素的文本方向设置

**8.<bdo></bdo>** 设置文字的显示方向，很少使用

```html
<p><bdo dir="rtl">1234<bdi>.5678.</bdi>9 10。</bdo></p>
<!-- 以上显示：。01 9.5678.4321 -->
```

**9.<blockquote></blockquote>** 定义一个引用块，浏览器通常会对其进行缩进

**提示：**如果引用仅仅是一句话，那么推荐用 q 标签

```html
<blockquote cite="http://www.worldwildlife.org/who/index.html">
For 50 years, WWF has been protecting the future of nature. The world's leading conservation organization, WWF works in 100 countries and is supported by 1.2 million members in the United States and close to 5 million globally.
</blockquote>
```

**10.<caption></caption>** 定义表格的标题，只能对每个表格定义一个标题

**提示：**必须直接放置到 table 标签之后

**11.<cite></cite>** 定义作品的标题（如书籍、歌曲、电影、电视节目、绘画、雕塑等）

```html
<p><cite>The Scream</cite> by Edward Munch. Painted in 1893.</p>
```

**12.<code></code>** 短语标签，定义计算机代码文本

```html
<code>一段电脑代码</code>
```

**13.<col/>** 存在于 colgroup 标签中，定义元素内部每一列的样式

```html
<table border="1">
  <colgroup>
      <!-- span表示列数，可以向整个列应用样式，而不需要重复为每个单元格或每一行设置样式 -->
    <col span="2" style="background-color:red">
    <col style="background-color:yellow">
  </colgroup>
  <tr>
    <th>ISBN</th>
    <th>Title</th>
    <th>Price</th>
  </tr>
  <tr>
    <td>3476896</td>
    <td>My first HTML</td>
    <td>$53</td>
  </tr>
</table>
```

**14.<colgroup></colgroup>** 用于对表格中的列进行组合，以便对其进行格式化

**提示：**只能在 table 元素之内，在任何一个 caption 元素之后，在任何一个 thead、tbody、tfoot、tr 元素之前

```html
<!-- 同13 -->
```

**15.<datalist></datalist> 5*** 用于规定 input 元素可能的选项值

```html
<input list="browsers">
 <!-- input 标签的 list 属性值为 datalist 标签的 id -->
<datalist id="browsers">
  <option value="Internet Explorer">
  <option value="Firefox">
  <option value="Chrome">
  <option value="Opera">
  <option value="Safari">
</datalist>
```

**16.<dd></dd>** 在定义列表中定义条目的定义部分

```html
<dl>
   <dt>计算机</dt>
   <dd>用来计算的仪器 ... ...</dd>
   <dt>显示器</dt>
   <dd>以视觉方式显示信息的装置 ... ...</dd>
</dl>
```

**17.<del></del>** 定义文档中已被删除的部分

```html
<p>
    a dozen is <del>20</del> 12 pieces
</p>
```

**18.<details></details>** 规定了用户可见的或者隐藏的需求的补充细节

```html
<details>
	<!-- summary 标签定义不可见时显示的内容 -->
<summary>Copyright 1999-2011.</summary>
<p> - by Refsnes Data. All Rights Reserved.</p>
<p>All content and graphics on this web site are the property of the company Refsnes Data.</p>
</details>
```

**19.<dfn></dfn>** 定义一个定义项目

```html
<!-- 对文本进行格式化 -->
<dfn>定义项目</dfn>
```

**20.<dialog></dialog>** 定义一个对话框、确认框或窗口

```html
<p>
    这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本
    <dialog open>这是对话框文本这是对话框文本这是对话框文本</dialog>
</p>
```

**21.<dl></dl>** 定义一个描述列表，标签与 dt（定义项目/名字）和 dd（描述每一个项目/名字）一起使用

```html
<dl>
  <dt>Coffee</dt>
    <dd>Black hot drink</dd>
  <dt>Milk</dt>
    <dd>White cold drink</dd>
</dl>
```

**22.<embed/>** 定义了一个容器，用来嵌入外部应用或者互动程序（插件）

```html
<embed src="helloworld.swf">
```

**23.<fieldset></fieldset>** 可以将表单内的相关元素分组

```html
<form>
  <fieldset>
    <legend>Personalia:</legend>
    Name: <input type="text"><br>
    Email: <input type="text"><br>
    Date of birth: <input type="text">
  </fieldset>
</form>
```

**24.<figcaption></figcaption>** 标签为 figure 元素定义标题

```html
<figure>
  <img src="img_pulpit.jpg" alt="The Pulpit Rock" width="304" height="228">
  <figcaption>Fig1. - A view of the pulpit rock in Norway.</figcaption>
</figure>
```

**25.<figure></figure>** 规定独立的流内容（图像、图表、照片、代码等）

```html
<figure>
  <img src="img_pulpit.jpg" alt="The Pulpit Rock" width="304" height="228">
</figure>
```

**26.<hgroup></hgroup>** 当标题有多个层级（副标题）时，<hgroup> 元素被用来对一系列 h1 - h6 元素进行分组

```html
<hgroup>
	<h1>Welcome to my WWF</h1>
	<h2>For a living planet</h2>
</hgroup>

<p>The rest of the content...</p>
```

**27.<iframe></iframe>** 规定一个内联框架，用来在当前 HTML 文档中嵌入另一个文档

```html
<iframe src="http://www.runoob.com"></iframe>
```

**28.<ins></ins>** 定义已经被插入文档中的文本

```html
<p>My favorite color is <del>blue</del> <ins>red</ins>!</p>
```

**29.<legend></legend>** 为 fieldset 元素定义标题

```html
<form>
  <fieldset>
    <legend>Personalia:</legend>
    Name: <input type="text" size="30"><br>
    Email: <input type="text" size="30"><br>
    Date of birth: <input type="text" size="10">
  </fieldset>
</form>
```

**29.<map></map>** 用于客户端图像映射

```html
<img src='' id='img'>
<map name='img'>
    <area shape='rect/circle' coords='映射坐标' href='' alt=''>
    <area>
    <area>
</map>
```

**30.<mark></mark> 5*** 定义带有记号的文本

```html
<p>Do not forget to buy <mark>milk</mark> today.</p>
```

**31.<meter></meter> 5*** 定义度量衡。仅用于已知最大和最小值的度量

**提示：**不能作为一个进度条来使用

```html
<meter value="2" min="0" max="10">2 out of 10</meter><br>
<meter value="0.6">60%</meter>
```

**32.<nav></nav> 5*** 定义导航条

```html
<nav>
  <a href="/html/">HTML</a> |
  <a href="/css/">CSS</a> |
  <a href="/js/">JavaScript</a> |
  <a href="/jquery/">jQuery</a>
</nav>
```

**33.<noscript></noscript>** 定义在脚本未被执行时的替代内容

**34.<optgrup></optgroup>** 用于把相关的选项组合在一起

```html
<select>
  <optgroup label="Swedish Cars">
    <option value="volvo">Volvo</option>
    <option value="saab">Saab</option>
  </optgroup>
  <optgroup label="German Cars">
    <option value="mercedes">Mercedes</option>
    <option value="audi">Audi</option>
  </optgroup>
</select>
```

**35.<output></output> 5*** 作为计算结果输出显示(比如执行脚本的输出)

```html
<form oninput="x.value=parseInt(a.value)+parseInt(b.value)">0
    <!-- output 标签的内容在 form 标签中的 oninput 属性定义 -->
  <input type="range" id="a" value="50">100
  +<input type="number" id="b" value="50">
  =<output name="x" for="a b"></output>
</form>
```

**36.<pre></pre>** 表示计算机的源代码

**37.<progress></progress> 5*** 定义运行中的任务进度

```html
<progress value="22" max="100"></progress>
```

**38.\<ruby>\</ruby> 5*** 

```html
<ruby>
  漢 <rp>(</rp><rt>han</rt><rp>)</rp>
  字 <rp>(</rp><rt>zi</rt><rp>)</rp>
</ruby>
```

**39.<s></s>** 对不正确、不准确或者没有用的文本进行标识

```html
<p><s>My car is blue.</s></p>
<p>My new car is silver.</p>
```

**40.<section></section> 5*** 定义了文档的某个区域，如章节、头部、底部或者文档的其他区域

**41.<small></small>** 定义小型文本

**42.<sub></sub>** 定义文本的下标

​	**<sup></sup>** 定义文本的上标

```html
<p>这个文本包含 <sub>下标</sub>文本。</p>
<p>这个文本包含 <sup>上标</sup> 文本。</p>
```

**43.<time></time> 5*** 定义公历的时间（24 小时制）或日期，时间和时区偏移是可选的

```html
<!-- 该元素能够以机器可读的方式对日期和时间进行编码，这样，举例说，用户代理能够把生日提醒或排定的事件添加到用户日程表中，搜索引擎也能够生成更智能的搜索结果。 -->
<p>我们在每天早上 <time>9:00</time> 开始营业。</p>
<p>我在 <time datetime="2016-02-14">情人节</time> 有个约会。</p>
```

**45.<wbr></wbr>** 规定在文本中的何处适合添加换行符

**提示：**如果单词太长，或者您担心浏览器会在错误的位置换行，那么您可以使用 wbr 元素来添加 Word Break Opportunity（单词换行时机）

