---
title: CSS布局
date: 2017-02-19 15:32:44
tags: [CSS,布局]
---
以前对CSS布局有些接触，但是因为没有系统的学习过，导致每次在调整和修改起来都觉得比较困难，为了解决这个难啃的骨头花了两天时间系统的把目前比较主流的CSS布局方式系统的看了一遍，解决了之前的一些模糊的知其然不知其所以然的问题，在此记录供后续参考。

## 一、盒模型（Box Model）

所有HTML元素可以看作盒子，CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：

* margin：边距/外边距，清除边框外的区域，外边距是透明的；
* border：边框，围绕在内边距和内容外的边框；
* padding：填充/内边距，清除内容周围的区域，内边距是透明的；
* content：内容，盒子的内容，显示文本和图像。

<!-- more -->

下面的图片展示了盒子模型：

![img](http://olywxnzqu.bkt.clouddn.com/img/css_buju/BoxModel.jpg)

**元素的高度和宽度**

通过CSS设置元素的高度和宽度，实际设置的是content（内容）部分的高度和宽度而不是整个盒子的宽度和高度，而整个盒子的宽度是：（内容宽度 +padding宽度 +  border宽度 + margin宽度）之和，如下图：

![img](http://olywxnzqu.bkt.clouddn.com/img/css_buju/boxmodel4.jpg)

这样我们改四个中的其中一个，都会导致盒子宽度的改变，这对我们来说不友好，为了消除这种差异，CSS新增了`box-sizing`属性，现在通过CSS设置的宽度300px是content+padding+border，如下图：

![img](http://olywxnzqu.bkt.clouddn.com/img/css_buju/boxmodel2.jpg)

想要页面上所有的元素在高度和宽度的设置上都有统一的表现，所以建议为系统写css时候，第一个样式是：

``` css
*{
  margin:0;
  padding:0;
  -webkit-box-sizing: border-box;		/*兼容webkit内核浏览器，如：Chrome，Safari等*/
     -moz-box-sizing: border-box;		/*兼容火狐浏览器*/
          box-sizing: border-box;
}
```

***

## 二、display属性

`display` 是CSS中最重要的用于控制布局的属性，决定元素属于那种盒子（指定元素的显示类型）。每个元素都有一个默认的 display 值，这与元素的类型有关。对于大多数元素它们的默认值通常是 `block` 或 `inline` 。

**块级元素（block）**

一般是其他元素的容器，可容纳内联元素和其他块状元素，块状元素排斥其他元素与其位于同一行，宽度(width)高度(height)起作用。常见块状元素为`div`、`p`、`h1`、`li`。

**内联元素（inline）**

内联元素只能容纳文本或者其他内联元素，它允许其他内联元素与其位于同一行，其宽度(width)和高度(height)只与其包含的内容有关，通过CSS设置高度和宽度不起作用。常见内联元素为`a`、`span`。

可以通过设置display属性让块级元素和内联元素相互转换，如：

``` css
li{
  display:inline;		/*将内联元素设置为块级元素*/
}

span{
  display:block;		/*将块级元素设置为内联元素*/
}
```

**隐藏元素（none）**

一些特殊元素的默认 display 值就是none，如：`script`。

隐藏一个元素可以通过`display:none`，或`visibility:hidden`。但是请注意，这两种方法会产生不同的结果：

> * 通过visibility:hidden隐藏的元素仍需占用与未隐藏之前一样的空间，也就是说，该元素虽然被隐藏了，但仍然会影响布局；
> * display:none，不会占据它本来应该占据的空间，不会影响布局。

**块级内联元素（inline-block）**

将对象呈递为内联对象，但是对象的内容作为块对象呈递。旁边的内联对象会被呈递在同一行内，允许空格。准确地说，应用此特性的元素呈现为内联对象，周围内联元素保持在同一行，但保留设置宽度和高度地块元素的属性。

***

## 三、position属性

display决定了盒子种类，position就用来确定盒子的位置。

**标准文档流（static）**

`static` 是positon属性的默认值，指按照文档至上而下的标准文档流，被 `position: static` 修饰的元素不会受到top, bottom, left, right影响。

**绝对定位（fixed）**

元素的位置相对于浏览器窗口固定，配合top、right、bottom、left进行定位，脱离标准文档流，不会受到父元素overflow:hidden影响，即使窗口是滚动的它也不会移动，常用示例：二维码、广告的悬浮；

**绝对定位（absolute）**

绝对定位的元素的位置相对于最近的已定位（position属性值不为static）*父元素*，如果元素没有已定位的父元素，那么它的位置相对于`body`，配合top、right、bottom、left进行定位，脱离标准文档流，会受到父元素`overflow:hidden`影响，并且它会随着页面滚动而移动。

**相对定位（relative）**

默认参照父级的原始点为原始点，配合top、right、bottom、left进行定位，当父级内有padding等CSS属性时，当前级的原始点则参照父级内容区的原始点进行定位。

***

## 四、float属性

元素的水平方向浮动，意味着元素只能左右移动而不能上下移动，一个浮动元素会尽量向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框的边框为止，浮动元素之后的元素将围绕它，浮动元素之前的元素将不会受到影响，其属性值如下：

* left（向左浮动）
* rigth（向右浮动）
* top（向上浮动）
* bottom（向下浮动）

**清除浮动（clean）**

使用浮动后会影响到后面的元素所以需要及时的清除浮动，而且会造成父级元素高度塌陷。

``` css
.clean{
  clean:left;	/*清除左浮动*/
  clean:right;	/*清除右浮动*/
  clean:top;	/*清除上浮动*/
  clean:bottom;	/*清除下浮动*/
  clean:both;	/*清除所有浮动*/
}
/*清除浮动并防止父级元素高度塌陷*/
.clearfix:before, .clearfix:after {
    content: '\0020';
    display: block;
    overflow: hidden;
    visibility: hidden;
    width: 0; height: 0;
}
.clearfix:after { clear: both }
.clearfix { *zoom: 1 }

```

## 五、响应式布局

使用**媒体查询 （@media）** 可以针对不同的媒体类型定义不同的样式，达到响应式布局的目的，在不同尺寸的设备上都有完美的展示。

``` css
@media screen and (min-width:600px) {
  nav {
    float: left;
    width: 25%;
  }
  section {
    margin-left: 25%;
  }
}

@media screen and (max-width:599px) {
  nav li {
    display: inline;
  }
}
```

同时可以针对不同的媒体引用不同的 *stylesheets* 

``` css
<link media="screen and (max-device-width:299px)" rel="stylesheet" href="small.css" />
<link media="screen and (min-device-width:300px) and (max-device-width:900px)" rel="stylesheet" href="middle.css" />
<link media="screen and (min-device-width:901px)" rel="stylesheet" href="big.css" />
```

## 六、弹性布局（Flex）

Flex是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。

采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`。图示如下：

![img](http://olywxnzqu.bkt.clouddn.com/img/css_buju/flex3.png)

任何一个容器都可以指定为flex布局
``` CSS
.box{
  display: -webkit-flex;
  display: flex;
}
```

行内元素也可以使用flex布局

``` CSS
.box{
  display: -webkit-inline-flex;
  display：inline-flex;
}
```

** 注意：** 设为Flex布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效，如果元素不是弹性盒模型对象的子元素，则 flex 属性不起作用。

下面了解一下弹性布局的威力：

``` css
.container {
  display: -webkit-flex;
  display: flex;
}
.initial {
  -webkit-flex: initial;
          flex: initial;
  width: 200px;
  min-width: 100px;
}
.none {
  -webkit-flex: none;
          flex: none;
  width: 200px;
}
.flex1 {
  -webkit-flex: 1;
          flex: 1;
}
.flex2 {
  -webkit-flex: 2;
          flex: 2;
}
```

![img](http://olywxnzqu.bkt.clouddn.com/img/css_buju/flex1.jpg)

实现棘手的垂直居中：

``` css
.vertical-container {
  height: 300px;
  display: -webkit-flex;
  display:         flex;			/*设置容器为flex布局*/
  -webkit-align-items: center;		/*设置交叉轴方向上的对齐方式为居中*/
          align-items: center;		
  -webkit-justify-content: center;	/*设置主轴方向上的对齐方式为居中*/
          justify-content: center;
}
```

![img](http://olywxnzqu.bkt.clouddn.com/img/css_buju/flex2.jpg)



对使用flex布局的容器的属性：

> * `flex-direction`		定义主轴的方向（即项目的排列方向）
> * `flex-wrap`         定义当一条轴线排不下时换行的方式
> * `flex-flow`         是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。
> * `justify-content`   定义了项目在主轴上的对齐方式。
> * `align-items`       定义项目在交叉轴上如何对齐。
> * `align-content`     定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

和使用flex布局容器里子元素的属性：

> * `order`		       定义项目的排列顺序。数值越小，排列越靠前，默认为0。
> * `flex-grow`      定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
> * `flex-shrink`    定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
> * `flex-basis`     定义了在分配多余空间之前，项目占据的主轴空间（main size），也可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间
> * `flex`           是 flex-grow、flex-shrink 和 flex-basis 属性的简写属性，默认值为`0 1 auto`后两个属性可选。该属性有三个快捷值：`auto` (`1 1 auto`) 、 none (`0 0 auto`)和initial（`0 1 auto`）。
> * `align-self`     允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性，默认值为`auto`，表示继承父元素的`align-items`属性。

因flex布局涉及到的属性比较多在此只做属性描述待深入学习后再做总结。

## 七、Grid布局

CSS Grid现在已经被**W3C**纳入到CSS3的一个布局模块当中，但由于其目前仅在IE10+上支持，而且也仅支持部分属性，目前各浏览器对其支持程度不一致，致使无法全面使用，就算是学习做一些测试示例都还需要做一些设置，所以暂时不做学习，等后续标准规范之后再学习总结。

不过“大漠”在[W3cplus](http://www.w3cplus.com/)中已经有了详细的教程，感兴趣的可以提前了解学习，链接如下：

[CSS Grid布局学习](http://www.w3cplus.com/css3/what-is-css-grid-layout.html)



## 八、参考资料

[学习CSS布局](http://zh.learnlayout.com/)

[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[css知多少（7）——盒子模型](http://www.cnblogs.com/wangfupeng1988/p/4287292.html)

[css布局-盒模型，定位和浮动](http://www.myronliu.com/2016/03/04/css/css%E5%B8%83%E5%B1%80-%E7%9B%92%E6%A8%A1%E5%9E%8B%EF%BC%8C%E5%AE%9A%E4%BD%8D%E5%92%8C%E6%B5%AE%E5%8A%A8/)
