**<table>**

```html
<table>
        <caption>This is a table</caption>
        <colgroup>
          <col span='2' class='skyblue'>
          <col span='2' class='crimson'>
        </colgroup>
        <thead>
          <tr>
            <th>One</th>
            <th>Two</th>
            <th>Three</th>
            <th>Four</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>1</td>
            <td>2</td>
            <td>3</td>
            <td>4</td>
          </tr>
          <tr>
            <td>1</td>
            <td>2</td>
            <td>3</td>
            <td>4</td>
          </tr>
          <tr>
            <td>1</td>
            <td>2</td>
            <td>3</td>
            <td>4</td>
          </tr>
        </tbody>
        <tfoot>
          <tr>
            <td>e1</td>
            <td>e2</td>
            <td>e3</td>
            <td>e4</td>
          </tr>
        </tfoot>
      </table>
```

**<template>**

该标签内的html内容不会被渲染，可用javascript在后期进行实例化渲染

**<textarea>**

多行文本输入框

**<time>**

表示24小时制时间或公历日期

**<title>**

文档的标题

**<track>**

媒体元素<audio>, <video>的子元素，用于指定时序文本字幕

```html
<audio src='xxxxx'>
	<track default kind='caption' srclang='en' src='xxx'>
</audio>
```

**<u>**

波浪线文本，尽量使用CSS达成该效果

**<ul>**

无序列表

**<var>**

用于表示变量的名称

**<video>**

视频控件

**<wbr\>**

在文本中标记一个位置，浏览器可以选择（不代表一定）在此处换行