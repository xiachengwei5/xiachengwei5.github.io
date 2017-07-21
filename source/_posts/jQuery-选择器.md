---
title: jQuery 选择器
date: 2017-01-07 20:32:26
tags: [jQuery, 选择器]
---
### 一、基本选择器

| 语法           | 说明                    |
| ------------ | --------------------- |
| $("*")       | 选择文档中的所有元素            |
| $("div")     | 元素选择器，选择所有的div元素，返回数组 |
| $("#id")     | Id选择器，返回单个元素          |
| $(".class")  | class选择器，返回数组         |
| $("p , div") | 并列选择器，返回所有的p元素和div元素  |
<!-- more -->
### 二、层次选择器

| 语法         | 说明                                       |
| ---------- | ---------------------------------------- |
| $("A  B")  | 后代选择器，选择所有A元素下面的所有B子元素，包含非直接子节点          |
| $("A > B") | 子元素选择器，选择A下面的直接B子元素，不包含非直接子节点            |
| $("A + B") | 紧邻兄弟元素选择器，选择A元素后面紧邻的B元素，如果没有不选中，等同于next()方法 |
| $("A ~ B") | 兄弟元素选择器，选择A后面所有的B元素，等同于nextAll()方法       |

### 三、过滤选择器

**1、基本过滤选择器**

| 语法                     | 说明                 |
| ---------------------- | ------------------ |
| $("span : first")      | 选取第一个元素            |
| $("span : last")       | 选取第二个元素            |
| $("span : not(.wrap)") | 取非元素               |
| $("tr : even")         | 选取偶数行（索引从0开始）      |
| $("tr : odd")          | 选取奇数行（索引从0开始）      |
| $("tr : eq(2)")        | 选取指定索引的元素（索引从0开始）  |
| $("tr : gt(2)")        | 选取大于索引号的元素（索引从0开始） |
| $("ul  li : lt(2)")    | 选取小于索引号的元素（索引从0开始） |
| $(" : header")         | 选取所有的标题元素          |
| $(" : animated")       | 选取所有的动画元素          |

**2、内容过滤选择器**

| 语法                            | 说明                         |
| ----------------------------- | -------------------------- |
| $("span : contains('hello')") | 选取包含具体文本的元素                |
| $("span : empty”)             | 选取不包含子元素或文本为空的元素           |
| $("ol  li : parent”)          | 选取包含子元素或文本不为空的元素           |
| $("div : has(span)”)          | 选取子元素含有指定元素的元素，不是直系子元素也会生效 |

**3、可见性过滤选择器**

| 语法                 | 说明                                       |
| ------------------ | ---------------------------------------- |
| $("div : hidden")  | 仅选取display:none或input type="hidden"的元素，不选取visibility: hidden或opacity:0的元素，也就是说:hidden只匹配那些“隐藏的”并且不占空间的元素 |
| $("div : visible") | 选取可见的元素                                  |

**4、属性过滤选择器**

| 语法                      | 说明                           |
| ----------------------- | ---------------------------- |
| $("[href]")             | 选取所有带有 href 属性的元素            |
| $("[href = '#']")       | 选取所有 href 属性的值等于 "#" 的元素     |
| $("[href != '#']")      | 选取所有 href 属性的值不等于 "#" 的元素    |
| $("[herf ^= 'http']")   | 选取所有 href 属性的值以 "http" 开头的元素 |
| $("[herf  \$= '.jsp']") | 选取所有 href 属性的值以 "jsp" 结尾的元素  |
| $("[herf *= 'www']")    | 选取所有 href 属性的值包含 "www" 的元素   |

### 四、表单选择器

**1、基本表单选择器**

| 语法             | 说明                                       |
| -------------- | ---------------------------------------- |
| $(":input")    | 选取所有的 `input` 元素                         |
| $(":text")     | 选取所有type="text"的 `input` 元素              |
| $(":password") | 选取所有type="password"的 `input` 元素          |
| $(":radio")    | 选取所有type="radio"的 `input` 元素             |
| $(":checkbox") | 选取所有type="checkbox"的 `input` 元素          |
| $(":submit")   | 选取所有type="submit"的 `input` 元素和`button`的元素 |
| $(":reset")    | 选取所有type="reset"的 `input` 元素和`button`的元素 |
| $(":button")   | 选取所有type="button"的 `input` 元素和所有标签为`button`的元素 |
| $(":image")    | 选取所有type="image"的 `input` 元素             |
| $(":file")     | 选取所有type="file"的 `input` 元素              |

**2、表单元素过滤选择器**

| 语法             | 说明                                       |
| -------------- | ---------------------------------------- |
| $(":enabled")  | 选择所有启用的 `input` 和 `button` 元素            |
| $(":disabled") | 选择所有禁用（即设置了disabled="disabled"）的 `input` 和 `button` 元素 |
| $(":selected") | 选择所有被选中**下拉列表** 选项                       |
| $(":checked")  | 选择所有被选中的**复选框** 或**单选** 按钮元素             |

### 五、jQuery CSS 选择器

jQuery CSS 选择器可用于改变 HTML 元素的 CSS 属性。

**语法：** jQuery选择器.css("css属性", "css属性值");

``` js
$("div").css("background-color","red");
```

### 六、jQuery查找父、子、兄弟节点的方法

| 语法                    | 说明                                       |
| --------------------- | ---------------------------------------- |
| jQuery.parent(expr)   | 找父节点，可以传入expr进行过滤，比如$("span").parent()或者\$("span").parent(".class") |
| jQuery.parents(expr)  | 查找所有祖先元素，从父元素开始查找                        |
| jQuery.closest(expr)  | 查找第一个匹配的祖先元素，从当前元素开始查找                   |
| jQuery.children(expr) | 返回所有子节点，这个方法只会返回直接的孩子节点，不会返回所有的子孙节点      |
| jQuery.contents()     | 返回下面的所有内容，包括节点和文本。                       |
| jQuery.prev()         | 返回上一个兄弟节点，不是所有的兄弟节点                      |
| jQuery.prevAll()      | 返回所有之前的兄弟节点                              |
| jQuery.next()         | 返回下一个兄弟节点，不是所有的兄弟节点                      |
| jQuery.nextAll()      | 返回所有之后的兄弟节点                              |
| jQuery.siblings()     | 返回兄弟姐妹节点，不分前后                            |
| jQuery.find(expr)     | 不会有初始集合中的内容，比如$("p"),find("span"),是从子元素中找，等同于\$("p span") |
| jQuery.filter(expr)   | 会有初始集合中的内容                               |

### 七、参考资料

[jQuery选择器大全](http://www.cnblogs.com/tylerdonet/archive/2013/04/02/2996713.html)

[jQuery选择器总结](http://www.cnblogs.com/onlys/articles/jQuery.html)

[w3school-jQuery 参考手册 - 选择器](http://www.w3school.com.cn/jquery/jquery_ref_selectors.asp)
