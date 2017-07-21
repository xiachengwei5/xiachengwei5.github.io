---
title: CSS选择器
date: 2017-02-12 17:39:34
tags: [CSS, 选择器]
---

CSS选择器是学习CSS的一个核心部分，HTML页面中的元素就是通过CSS选择器来进行控制的，熟练使用CSS选择器能迅速根据文档结构对文档样式进行一对一，一对多或者多对一的控制和调整。

---

### 一、CSS 元素选择器（类型选择器）

文档的元素是最基本的选择器也是最常见的选择器，如果设置 HTML 的样式，选择器通常将是某个 HTML 元素，比如 `p`、`h1`、`em`、`a`，甚至可以是 `html `本身，如：

``` css
html {margin: 0; padding: 0;}
h1 {font-family: "华文楷体"; color:blue;}
a {font-size:16px; text-decoration: none;}
```
<!-- more -->
在 W3C 标准中，元素选择器又称为类型选择器（type selector）。**类型选择器匹配文档语言元素类型的名称。类型选择器匹配文档树中该元素类型的每一个实例**，所以类似HTML, XML等标记语言都可以通过类型选择器来匹配，甚至一些自定义标签都会生效。


### 二、CSS 类选择器

类选择器允许以一种独立于文档元素的方式来指定样式。该选择器可以单独使用，也可以与其他元素结合使用。

**提示：** 只有适当地标记文档后（*指定claess属性*），才能使用这些选择器，所以使用这两种选择器通常需要先做一些构想和计划。**要应用样式而不考虑具体设计的元素，最常用的方法就是使用类选择器**。

**语法：** 类选择器前面有个"."。

``` css
/* 基本语法 */
.important {color:red;}
/* 结合通配符使用 */
*.important {color:red;}
/* 结合元素使用，解释为：其 class 属性值为 important 的所有 p 元素 */
p.important {color:red;}
```

**CSS多类选择器**

HTML元素的class属性可以包含一个词列表，各个词之间用空格分隔，*词的顺序无关紧要*。如：

``` html
<p class="important">
	This paragraph is a very important.
</p>

<p class="warning">
	This paragraph is a very warning.
</p>

<p class="important warning">
	This paragraph is a very important warning.
</p>
```

设置class 为 important 的所有元素都是粗体，而 class 为 warning 的所有元素为斜体，class 中同时包含 important 和 warning 的所有元素还有一个银色的背景 。就可以写作：

``` css
.important {font-weight:bold;}
.warning {font-style:italic;}
/* 注意：在HTML元素的class属性中的两个词需要用“空格”隔开，在CSS中不能用“空格”隔开 */
.important.warning {background:silver;}
```

**注意：** 把两个类选择器链接在一起，仅可以选择**同时**包含这些类名的元素（类名的顺序不限），如果一个多类选择器包含类名列表中没有的一个类名，匹配就会失败。如：

``` css
p.important.urgent {background:silver;}
```

这个选择器将只匹配 class 属性中同时包含词 important 和 urgent 的 p 元素。因此，如果一个 p 元素的 class 属性中只有词 important 和 warning，将不能匹配。不过，它能匹配以下元素：

``` html
<!-- 没有同时包含 important 和 urgent ，不能匹配 -->
<p class="important warning">
	This paragraph is a very important.
</p>

<!-- 同时包含了 important 和 urgent ，可以匹配，与出现的顺序无关 -->
<p class="important warning urgent">
	This paragraph is a very important.
</p>
```



### 三、CSS ID选择器

**语法：** ID选择器前面有个"#"。

``` css
/* 基本语法 */
#intro {font-weight:bold;}
/* 配合通配符使用 */
*#intro {font-weight:bold;}
```

**与Javascript的区别：**

``` css
/* 在同一个页面中能匹配所有 ID 为 mostImportant 的元素 */
#mostImportant {color:red; background:yellow;}
```

``` javascript
/* 在同一个页面中只能获取文档中第一个 ID 为 mostImportant 的内容 */
document.getElementById("mostImportant").innerHTML;
```

``` html
<!-- 以下内容在同一个html文件中，当然这不符合标准，此处仅用于说明区别 -->
<h1 id="mostImportant">This is important!</h1>
<em id="mostImportant">This is important!</em>
<ul id="mostImportant">This is important!</ul>
```

**与类选择器的区别：**

*区别 1：*只能在文档中使用一次
不同于类选择器，在一个 HTML 文档中，ID 选择器会使用一次，而且仅一次。

*区别 2：*不能使用 ID 词列表
不同于类选择器，ID 选择器不能结合使用，因为 ID 属性不允许有以空格分隔的词列表。

**注意：**

> * 关于命名：`id`和`class`属性不要以数字开头，数字开头的ID在 Mozilla/Firefox 浏览器中不起作用；
> * 关于大小写：`id`和`class`在HTML和XHTML中是区分大小的。



### 四、CSS 属性选择器

**CSS 2** 引入了属性选择器。属性选择器可以根据元素的*属性*及*属性值*来选择元素。

**简单属性选择：**

``` css
/* 通过单个属性选择 */
*[title] {color:red;}
a[href] {color:red;}

/* 通过多个属性选择，注意：匹配同时含有多个属性的元素 */
a[href][title] {color:red;}
img[alt][title] {border: 5px solid red;}
```

**根据完全匹配的属性值选择：**

``` css
/* 通过单个属性值选择 */
a[href="http://xiachengwei5.coding.me"] {color: red;}

/* 通过多个属性值选择 */
a[href="http://xiachengwei5.coding.me"][title="颭的博客"] {color: red;}
```

**根据部分属性值选择**

| 选择器                 | 描述                                       |
| :-------------------- | ---------------------------------------- |
| [attribute]           | 用于选取带有指定属性的元素。                           |
| [attribute = value]   | 用于选取带有指定属性和值的元素。                         |
| [attribute ~= value]  | 用于选取属性值中包含指定词汇的元素，该值匹配是*没有空格的字符串*。       |
| [attribute &#124;= value]  | 用于选取带有以指定值开头的元素，该值匹配的是value或value-。 |
| [attribute ^= value]  | 匹配属性值以指定值开头的每个元素，该值可以是任意值，**推荐使用**。      |
| [attribute $= value]  | 匹配属性值以指定值结尾的每个元素。                        |
| [attribute *= value]  | 匹配属性值中包含指定值的每个元素，该值可以是任意值，**推荐使用**。      |


### 五、CSS 后代选择器（包含选择器）

后代选择器可以选择作为某元素后代的元素。

**语法：** 父元素和后代元素从左至右排列，中间用**空格**隔开。

``` css
/* 匹配 h1 下所有的 em 元素 */
h1 em {color:red;}

/* 匹配所有 class 为 "maincontent" 的 div下所有的 a 元素 */
div.maincontent a {color:blue;}
```

``` html
<!-- 匹配成功 -->
<h1>This is a <em>important</em> heading</h1>

<!-- 匹配失败 -->
<p>This is a <em>important</em> paragraph.</p>
```

**注意：** 有关后代选择器有一个易被忽视的方面，即两个元素之间的层次间隔可以是**无限**的。

例如，如果写作 `ul em`，这个语法就会选择从 ul 元素继承的所有 em 元素，而不论 em 的嵌套层次多深。

因此，`ul em` 将会选择以下标记中的所有 em 元素：

``` html
<ul>
  <!-- 此处会匹配 -->
  <em>0</em>
  <li>List item 1
    <ol>
      <li>List item 1-1</li>
      <li>List item 1-2</li>
      <li>List item 1-3
        <ol>
          <li>List item 1-3-1</li>
          <!-- 此处也会匹配 -->
          <li>List item <em>1-3-2</em></li>
          <li>List item 1-3-3</li>
        </ol>
      </li>
      <li>List item 1-4</li>
    </ol>
  </li>
  <li>List item 2</li>
  <li>List item 3</li>
</ul>
```



### 六、CSS 子元素选择器

与后代选择器相比，子元素选择器（Child selectors）只能选择作为某元素子元素的元素。

如果你不希望选择任意的后代元素，而是希望缩小范围，只选择某个元素的子元素，那就要使用子元素选择器。

**后代元素：** 所有由其包裹的元素；

**子代元素：** 由其包裹，层级结构只比其小一级的元素；

**语法：** 父元素和子元素从左至右排列，中间用 **>** 隔开。

``` css
/* 选择只作为 h1 元素子元素的 strong 元素 */
h1 > strong {color:red;}
```

``` html
<!-- 匹配成功 -->
<h1>This is <strong>very</strong> <strong>very</strong> important.</h1>
<!-- 匹配失败 -->
<h1>This is <em>really <strong>very</strong></em> important.</h1>
```

**结合后台选择器和子选择器**

``` css
/* 选择所有 class 为 "company" 的 table 的所有后代 td 的子元素 p */
table.company td > p
```



### 七、CSS 相邻兄弟选择器

可选择紧接在另一元素后的元素，且二者有相同父元素。

**语法：**兄弟元素从左至右排列，中间用 **+** 隔开。

``` css
/* 选择紧接在 h1 后（前面的元素不能匹配）的元素，而且二者有相同的父元素 */
h1 + p {color: red;}
```

``` html
<!-- 匹配失败 -->
<p>第一个段落</p>
<h1>标题</h1>
<!-- 匹配成功 -->
<p>第二个段落</p>
<!-- 匹配失败 -->
<p>第二个段落</p>
```

**总结：**

| 选择器      | 描述                                 |
| -------- | ---------------------------------- |
| h1  p    | 后代选择器，选择所有的后代元素。                   |
| h1  ,  p | 多元素选择器，用","分隔，同时匹配元素h1或元素p。        |
| h1  >  p | 子元素选择器，选择其子元素。                     |
| h1  +  p | 相邻兄弟选择器，选择其**后面**相邻的兄弟元素（有相同的父元素）。 |
| h1  ~  p | 一般兄弟选择器，选择其**后面**所有的兄弟元素（有相同的父元素）。 |



### 八、CSS 伪类

用于向某些选择器添加特殊的效果。

**语法：** 选择器和伪类之间通过 **:** 隔开。

``` css
/* 选择器 : 伪类 */
selector : pseudo-class {property: value}
/* CSS 类也可与伪类搭配使用 */
selector.class : pseudo-class {property: value}
```

**锚伪类**

``` css
a:link {color: #FF0000}		/* 未访问的链接 */
a:visited {color: #00FF00}	/* 已访问的链接 */
a:hover {color: #FF00FF}	/* 鼠标移动到链接上 */
a:active {color: #0000FF}	/* 选定的链接 */
```

> * 在 CSS 定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。
> * 在 CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。
> * 伪类名称对大小写不敏感。

**:first-child 伪类和:laft-child伪类**

可以使用 :first-child 伪类来选择元素的第一个子元素。

``` html
<div>
  <!-- 第一个 p 元素 -->
  <p>These are the necessary steps:</p>
	<ul>
         <!-- 第一个 li 元素 -->
		<li>Intert Key</li>
		<li>Turn key
            <!-- 第一个 strong 元素 -->
            <strong>clockwise</strong>
         </li>
		<li>Push accelerator</li>
	</ul>
     <!-- 最后一个 p 元素 -->
	<p>Do
      <!-- 第一个 em 元素 -->
      <em>not</em>
      push the brake at the same time as the accelerator.
    </p>
</div>
```

``` css
/* 选择的就是第一个 p 元素，而不是 p 元素的第一个子元素 */
p:first-child {font-weight: bold;}
/* 选择的就是最后一个 p 元素，而不是 p 元素的最后一个子元素 */
p:last-child {font-weight: bold;}
/* 选择的就是第一个 li 元素，而不是 li 元素的第一个子元素 */
li:first-child {text-transform: uppercase;}
/* 通过CSS设置字母的大小写 */
li:first-child {text-transform: lowercase;}
```

**伪类汇总**

| **选择器**               | **含义**                                   |
| --------------------- | ---------------------------------------- |
| E:first-child         | 匹配元素E的第一个子元素                             |
| E:link                | 匹配所有未被点击的链接                              |
| E:visited             | 匹配所有已被点击的链接                              |
| E:active              | 匹配鼠标已经其上按下、还没有释放的E元素                     |
| E:hover               | 匹配鼠标悬停其上的E元素                             |
| E:focus               | 匹配获得当前焦点的E元素                             |
| E:lang(c)             | 匹配lang属性等于c的E元素                          |
| E:enabled             | 匹配表单中可用的元素                               |
| E:disabled            | 匹配表单中禁用的元素                               |
| E:checked             | 匹配表单中被选中的radio或checkbox元素                |
| E::selection          | 匹配用户当前选中的元素                              |
| E:root                | 匹配文档的根元素，对于HTML文档，就是HTML元素               |
| E:nth-child(n)        | 匹配其父元素的第n个子元素**，第一个编号为1**                |
| E:nth-last-child(n)   | 匹配其父元素的倒数第n个子元素，**第一个编号为1**              |
| E:nth-of-type(n)      | 与:nth-child()作用类似，但是仅匹配使用同种标签的元素         |
| E:nth-last-of-type(n) | 与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素   |
| E:last-child          | 匹配父元素的最后一个子元素，等同于:nth-last-child(1)      |
| E:first-of-type       | 匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1)   |
| E:last-of-type        | 匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1) |
| E:only-child          | 匹配父元素下仅有的一个子元素，等同于:first-child:last-child或 :nth-child(1):nth-last-child(1) |
| E:only-of-type        | 匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1) |
| E:empty               | 匹配一个不包含任何子元素的元素，文本节点也被看作子元素              |
| E:not(selector)       | 匹配不符合当前选择器的任何元素                          |



**伪元素**

伪元素可用于定位文档中包含的文本，为与伪类进行区分，伪元素使用双冒号 :: 定义，但单冒号 : 也能被识别。

| 属性             | 描述               |
| -------------- | ---------------- |
| ::first-letter | 向文本的第一个字母添加特殊样式。 |
| ::first-line   | 向文本的首行添加特殊样式。    |
| ::before       | 在元素之前添加内容。       |
| ::after        | 在元素之后添加内容。       |
| ::focue        | 在元素之后添加内容。       |

### 九、参考资料

[W3School  CSS 选择器](http://www.w3school.com.cn/css/css_selector_type.asp)

[CSS选择器详解](http://www.uisdc.com/css-selector)

[十分钟搞定CSS选择器](http://www.cnblogs.com/dolphinX/p/3347713.html)
