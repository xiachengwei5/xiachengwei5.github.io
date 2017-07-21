---
title: CSS3 新特性
date: 2017-02-25 13:49:29
tags: [CSS, CSS3]
---
CSS3 是最新的 CSS 标准，并且完全向后兼容，不过目前W3C 仍然在对 CSS3 规范进行开发，虽然标准的规范还没有正式发布，但是现代浏览器已经支持相当多的 CSS3 属性了。CSS3 提供了很多可以把玩的新特性，模糊了之前只控制样式的定义，让之前很难处理的样式（如：圆角、多列等）和只能通过Javascript来实现的动画效果等现在都能通过CSS3 新特性提供的属性很轻松的实现，功能是越来越强大。

### 一、CSS3 边框

在 css3 中新增的边框属性如下：

**创建圆角边框** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_border-radius)

在CSS2中添加圆角很棘手，我们不得不在每个角落使用不同的图像。但是在CSS3中通过添加一个属性就可以搞定，那就是`border-radius` ，代码如下：
<!-- more -->
**语法：** border-radius : length length;

length： 由浮点数字和单位标识符组成的长度值（如：20px）。不可为负值，如果为负值则与0展示效果一样。第一个值设置其水平半径，第二个值设置其垂直半径，如果第二个值省略则默认第二个值等于第一个值。

``` css
div{
  border: 1px solid;
  /* 设置每个圆角水平半径和垂直半径都为30px */
  border-radius: 30px;
}
```

`border-radius` 是4个角的缩写方法。四个角的表示顺序与`border`类似按照`border-top-left-radius`、`border-top-right-radius`、`border-bottom-right-radius`、`border-bottom-left-radius`的顺序来设置：

``` css
div{
  border: 1px solid;
  /* 如果 / 前后的值都存在，那么 / 前面的值设置其水平半径，/ 后面值设置其垂直半径，如果没有 / ，则水平和垂直半径相等 */
  border-radius: 10px 15px 20px 30px / 20px 30px 10px 15px;

  /* 上面写法等价于下面的写法，第一个值是水平半径，第二个值是垂直半径 */
  border-top-left-radius: 10px 20px;
  border-top-right-radius: 15px 30px;
  border-bottom-right-radius: 20px 10px;
  border-bottom-left-radius: 30px 15px;
}
```

`border-radius` 指定不同数量的值遵循对角相等的原则，即指定了值的取指定值，没有指定值的与对角值相等，对角相等模型如下：

![对角相等模型](http://olywxnzqu.bkt.clouddn.com/img/CSS3_xintexing/duijiaoxiangdeng.png)


**边框阴影**

通过属性`box-shadow` 向边框添加阴影。

**语法：** {box-shadow : [inset]  x-offset  y-offset  blur-radius  extension-radius  spread-radiuscolor}

说明：对象选择器 {box-shadow:[投影方式]  X轴偏移量  Y轴偏移量   模糊半径  阴影扩展半径 阴影颜色}

``` css
div{
  /* 内阴影，向右偏移10px，向下偏移10px，模糊半径5px，阴影缩小10px */
  box-shadow: inset 10px 10px 5px -10px #888888;
}
```

**box-shadow属性的参数设置取值：**

> 阴影类型：此参数可选。如不设值，默认投影方式是外阴影；如取其唯一值**inset** ，其投影为内阴影；
>
> X-offset:阴影水平偏移量，其值可以是正负值。如果值为正值，则阴影在对象的右边，其值为负值时，阴影在对象的左边；
>
> Y-offset:阴影垂直偏移量，其值也可以是正负值。如果为正值，阴影在对象的底部，其值为负值时，阴影在对象的顶部；
>
> Blur-radius:阴影模糊半径：此参数可选，但其值只能是为正值，如果其值为0时，表示阴影不具有模糊效果，其值越大阴影的边缘就越模糊；
>
> Extension-radius阴影扩展半径：此参数可选，其值可以是正负值，如果值为正，则整个阴影都延展扩大，反之值为负值时，则缩小；
>
> 阴影颜色：此参数可选。如不设定颜色，浏览器会取默认色，但各浏览器默认取色不一致，特别是在webkit内核下的safari和chrome浏览器下表现为透明色，在Firefox/Opera下表现为黑色（已验证），建议不要省略此参数。

**边框图片**

**语法：**
> * border-image : border-image-source || border-image-slice [ / border-image-width] || border-image-repeat

> * border-image ： none | image [ number | percentage]{1,4} [ / border-width>{1,4} ] ? [ stretch | repeat | round ]{0,2}

``` css
div{
  border-image:url(border.png) 30 30 round;

  border-image: url(border.png) 20/10px repeat;
}
```

border-image的参数说明：
> * none:是border-image的默认值，如果取值为none时，表示边框无背景图片；
> * image：设置border-image的背景图片，这个跟background-image一样，使用绝对或相对的url地址，来指定背景图片；
> * number：number是一个数值，用来设置边框的宽度，其单位是px，其实就像border-width一样取值，可以使用1~4个值，其具体表示四个方位的值，可以参考border-width的设置方式；
> * percntage：percntage也是用来设置边框的宽度，跟number不同之处是，其使用的是百分比值来设置边框宽度；
> * stretch,repeat,round:他们是用来设置边框背景图片的铺放方式，类似于background-position，其中stretch是拉伸，repeat是重复，round是平铺，stretch为默认值。
`border-image-slice` 这玩意比较复杂，感觉是看明白了，但是就是做不出例子的效果，不知道是资源图片的原因还是写法的原因，还是浏览器的原因还是写例子的人自己都没整明白，这东西现在比较糊，到时整明白在整理。



### 二、CSS3 背景

**`background-size` 属性**  [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_background-size)

在 CSS3 之前，背景图片的尺寸是由图片的实际尺寸决定的。在 CSS3 中，可以设置背景图片的尺寸，这就允许我们在不同的环境中重复使用背景图片。可以像素或百分比规定尺寸。如果以百分比规定尺寸，那么尺寸相对于父元素的宽度和高度。

``` css
div{
  background:url(bg_flower.gif);
  /* 通过像素规定尺寸 */
  background-size:63px 100px;

  /* 通过百分比规定尺寸 */
  background-size:100% 50%;
  background-repeat:no-repeat;
}
```

**`background-origin` 属性** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_background-origin)

规定背景图片的定位区域，背景图片可以放置于 `content-box`、`padding-box` 或 `border-box` 区域，

``` css
div{
  background:url(bg_flower.gif);
  background-repeat:no-repeat;
  background-size:100% 100%;
  /* 规定背景图片的定位区域 */
  background-origin:content-box;
}
```

区域的具体划分如下：

![background-origin 属性指定的区域](http://olywxnzqu.bkt.clouddn.com/img/CSS3_xintexing/background-origin.gif)

**`background-clip` 属性** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_background-clip)

与`background-origin` 属性相似，规定背景颜色的绘制区域，区域划分与`background-origin` 属性相同。

``` css
div{
  background-color:yellow;
  background-clip:content-box;
}
```

**CSS3 多重背景图片**   [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_background_multiple)

CSS3 允许为元素设置多个背景图像

``` css
body{
  background-image:url(bg_flower.gif),url(bg_flower_2.gif);
}
```

### 三、CSS3 文本效果

**`text-shadow` 属性** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_text-shadow)

给为本添加阴影，能够设置水平阴影、垂直阴影、模糊距离，以及阴影的颜色。

``` css
h1{
  text-shadow: 5px 5px 5px #FF0000;
}
```

**text-wrap 属性**  [示例](http://www.w3school.com.cn/cssref/pr_text-wrap.asp)

设置区域内的自动换行。

**语法：** text-wrap: normal | none | unrestricted | suppress | break-word;

``` css
/* 允许对长单词进行拆分，并换行到下一行 */
p {word-wrap:break-word;}
```

| 值          | 描述                              |
| ---------- | ------------------------------- |
| normal     | 只在允许的换行点进行换行。                   |
| none       | 不换行。元素无法容纳的文本会溢出。               |
| break-word | 在任意两个字符间换行。                     |
| suppress   | 压缩元素中的换行。浏览器只在行中没有其他有效换行点时进行换行。 |

### 四、CSS3 字体

**字体定义**

在 CSS3 之前，web 设计师必须使用已在用户计算机上安装好的字体。但是通过 CSS3，web 设计师可以使用他们喜欢的任意字体。当找到或购买到希望使用的字体时，可将该字体文件存放到 web 服务器上，它会在需要时被自动下载到用户的计算机上。字体需要在 CSS3 @font-face 规则中定义。

``` CSS
/* 定义字体 */
@font-face{
  font-family: myFont;
  src: url('Sansation_Light.ttf'),
       url('Sansation_Light.eot');     /* IE9+ */
}

div{
  font-family:myFont;
}
```

**使用粗体字体**

 "Sansation_Light.ttf"文件 是定义的正常字体，"Sansation_Bold.ttf" 是另一个字体文件，它包含了 Sansation 字体的粗体字符。只要 font-family 为 "myFirstFont" 的文本需要显示为粗体，浏览器就会使用该字体。

（其实没弄清楚这样处理的原因，经测试直接在html中通过 b 标签也可以实现加粗的效果）

``` css
/* 定义正常字体 */
@font-face{
  font-family: myFirstFont;
  src: url('/example/css3/Sansation_Light.ttf'),
       url('/example/css3/Sansation_Light.eot'); /* IE9+ */
}

/* 定义粗体时使用的字体 */
@font-face{
  font-family: myFirstFont;
  src: url('/example/css3/Sansation_Bold.ttf'),
       url('/example/css3/Sansation_Bold.eot'); /* IE9+ */
  /* 标识属性 */
  font-weight:bold;
}

div{
  font-family:myFirstFont;
}
```

### 五、CSS3 2D 转换

通过 CSS3 转换，我们能够对元素进行**移动、缩放、转动、拉长或拉伸**，转换是使元素改变形状、尺寸和位置的一种效果。

**translate() 方法** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transform_rotate)

通过 translate(x , y) 方法，元素根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数从其当前位置移动，x为正值向右移动，为负值向左移动；y为正值向下移动，为负值向上移动；

``` css
div{
  transform: translate(50px,100px);
  -ms-transform: translate(50px,100px);		   /* IE 9 */
  -webkit-transform: translate(50px,100px);     /* Safari and Chrome */
  -o-transform: translate(50px,100px);		   /* Opera */
  -moz-transform: translate(50px,100px);        /* Firefox */
}
```

**rotate() 方法** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transform_rotate)

控制元素顺时针旋转给定的角度。为正值，元素将顺时针旋转。为负值，元素将逆时针旋转。

``` css
div{
  transform: rotate(30deg);
  -ms-transform: rotate(30deg);        /* IE 9 */
  -webkit-transform: rotate(30deg);    /* Safari and Chrome */
  -o-transform: rotate(30deg);         /* Opera */
  -moz-transform: rotate(30deg);       /* Firefox */
}
```

**scale() 方法** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transform_scale)

根据给定的宽度（X 轴）和高度（Y 轴）参数，控制元素的尺寸的增加、减少。

``` css
div{
  transform: scale(2,4);
  -ms-transform: scale(2,4);         /* IE 9 */
  -webkit-transform: scale(2,4);     /* Safari 和 Chrome */
  -o-transform: scale(2,4);	        /* Opera */
  -moz-transform: scale(2,4);       /* Firefox */
}
```

**skew() 方法** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transform_skew)

根据给定的水平线（X 轴）和垂直线（Y 轴）参数设置元素翻转给定的角度。

``` css
/* 设置围绕 X 轴把元素翻转 30 度，围绕 Y 轴翻转 20 度。 */
div{
  transform: skew(30deg,20deg);
  -ms-transform: skew(30deg,20deg);         /* IE 9 */
  -webkit-transform: skew(30deg,20deg);     /* Safari and Chrome */
  -o-transform: skew(30deg,20deg);          /* Opera */
  -moz-transform: skew(30deg,20deg);        /* Firefox */
}
```

**matrix() 方法** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transform_matrix)

matrix() 方法把所有 2D 转换方法组合在一起。matrix() 方法需要六个参数，包含数学函数，允许旋转、缩放、移动以及倾斜元素。

``` css
/* 使用 matrix 方法将 div 元素旋转 30 度 */
div{
  transform:matrix(0.866,0.5,-0.5,0.866,0,0);
  -ms-transform:matrix(0.866,0.5,-0.5,0.866,0,0);          /* IE 9 */
  -moz-transform:matrix(0.866,0.5,-0.5,0.866,0,0);         /* Firefox */
  -webkit-transform:matrix(0.866,0.5,-0.5,0.866,0,0);      /* Safari and Chrome */
  -o-transform:matrix(0.866,0.5,-0.5,0.866,0,0);           /* Opera */
}
```

**2D Transform 方法汇总**

| 函数                    | 描述                       |
| --------------------- | ------------------------ |
| matrix(n,n,n,n,n,n)   | 定义 2D 转换，使用六个值的矩阵。       |
| translate(x,y)        | 定义 2D 转换，沿着 X 和 Y 轴移动元素。 |
| translateX(n)         | 定义 2D 转换，沿着 X 轴移动元素。     |
| translateY(n)         | 定义 2D 转换，沿着 Y 轴移动元素。     |
| scale(x,y)            | 定义 2D 缩放转换，改变元素的宽度和高度。   |
| scaleX(n)             | 定义 2D 缩放转换，改变元素的宽度。      |
| scaleY(n)             | 定义 2D 缩放转换，改变元素的高度。      |
| rotate(angle)         | 定义 2D 旋转，在参数中规定角度。       |
| skew(x-angle,y-angle) | 定义 2D 倾斜转换，沿着 X 和 Y 轴。   |
| skewX(angle)          | 定义 2D 倾斜转换，沿着 X 轴。       |
| skewY(angle)          | 定义 2D 倾斜转换，沿着 Y 轴。       |

### 六、CSS3 3D 转换

CSS3 允许使用 3D 转换来对元素进行格式化

**rotateX() 方法** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transform_rotateX)

``` css
/* 设置元素围绕其 X 轴以给定的度数进行旋转 */
div{
  transform: rotateX(120deg);
  -webkit-transform: rotateX(120deg);	/* Safari 和 Chrome */
  -moz-transform: rotateX(120deg);	/* Firefox */
}
```

**rotateY() 旋转** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transform_rotateY)

``` css
/* 设置元素围绕其 Y 轴以给定的度数进行旋转 */
div{
  transform: rotateY(130deg);
  -webkit-transform: rotateY(130deg);	/* Safari 和 Chrome */
  -moz-transform: rotateY(130deg);	/* Firefox */
}
```

2017百度前端学院热身试题 [百度前端学院热身试题-第三题](http://ife.baidu.com/game/warmUp#/F4A83299B6A3990C231A5693E52A25A1BB3AAB) 就重点考察了元素2D、3D转换的内容，说明这部分是比较适用比较重要的部分，要熟练掌握。

![百度前端学院热身试题-第三题](http://olywxnzqu.bkt.clouddn.com/img/CSS3_xintexing/baiduxueyuan.png)

### 七、CSS3 过渡

通过 CSS3可以在不使用 Flash 动画或 JavaScript 的情况下，当元素从一种样式变换为另一种样式时为元素添加效果。

要实现这一点，必须规定以下两项内容：

* 设置添加过渡效果的 CSS 属性；
* 设置过渡效果的时长；

**注意：** 如果时长未设置，则不会有过渡效果，因为默认值是 0。

**单项改变** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transition1)

``` css
/* 设置将变化效果添加在“宽度”上，时长为2秒；该时长在其他属性上并不适用 */
div{
  transition: width 2s;
  -moz-transition: width 2s;         /* Firefox 4 */
  -webkit-transition: width 2s;      /* Safari 和 Chrome */
  -o-transition: width 2s;           /* Opera */
}
/* 配合在一起使用的效果就是当鼠标移上去的时候宽度变为300px，这个过程耗时2秒 */
div:hover{
  width:300px;
}
```

**注意：** 当鼠标移出元素时，它会逐渐变回原来的样式。

**多项改变** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_transition2)  

如需向多个样式添加过渡效果，请添加多个属性，由逗号隔开。

``` css
/* 同时向宽度、高度和转换添加过渡效果 */
div{
  transition: width 2s, height 2s, transform 2s;
  -moz-transition: width 2s, height 2s, -moz-transform 2s;
  -webkit-transition: width 2s, height 2s, -webkit-transform 2s;
  -o-transition: width 2s, height 2s,-o-transform 2s;
}

/* 当鼠标移上时宽度和高度都变成200px，同时旋转180度，每个属性变化都耗时2秒 */
div:hover{
  width:200px;
  height:200px;
  transform:rotate(180deg);
  -moz-transform:rotate(180deg);        /* Firefox 4 */
  -webkit-transform:rotate(180deg);     /* Safari and Chrome */
  -o-transform:rotate(180deg);          /* Opera */
}
```

**过渡属性详解** [详解](http://www.w3school.com.cn/css3/css3_transition.asp)

`transition` 是简写属性，

**语法：** transition : transition-property | transition-duration | transition-timing-function | transition-delay;

``` css
/* 设置在宽度上添加过渡效果，时长为1秒，过渡效果时间曲线为linear，等待2秒后开始过渡 */
div{
  transition: width 1s linear 2s;
  -moz-transition: width 1s linear 2s;       /* Firefox 4 */
  -webkit-transition: width 1s linear 2s;    /* Safari and Chrome */
  -o-transition: width 1s linear 2s;         /* Opera */
}
```

| 属性                         | 描述                      |
| -------------------------- | ----------------------- |
| transition                 | 简写属性，用于在一个属性中设置四个过渡属性。  |
| transition-property        | 规定应用过渡的 CSS 属性的名称。      |
| transition-duration        | 定义过渡效果花费的时间。默认是 0。      |
| transition-timing-function | 规定过渡效果的时间曲线。默认是 "ease"。 |
| transition-delay           | 规定过渡效果何时开始。默认是 0。       |

### 八、CSS3 动画

通过 CSS3可以创建动画，这些动画可以取代网页中的画图片、Flash 动画以及 JavaScript。

CSS3 中通过@keyframes 规则来创建动画。在 @keyframes 中规定某项 CSS 样式，就能创建由当前样式（动画开始前的样式）逐渐改为新样式（需要变到的样式）的动画效果。

**通过from , to关键字设置动画发生的时间** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_animation1)  

``` css
/* 通过@keyframes 创建动画 */
@keyframes myfirst{
  from {background: red;}
  to {background: yellow;}
}
/* Firefox */
@-moz-keyframes myfirst {
  from {background: red;}
  to {background: yellow;}
}
/* Safari 和 Chrome */
@-webkit-keyframes myfirst {
  from {background: red;}
  to {background: yellow;}
}
/* Opera */
@-o-keyframes myfirst {
  from {background: red;}
  to {background: yellow;}
}

/*
   将创建的动画绑定到选择器，并至少指定以下两项 CSS3 动画属性
   1.指定动画的名称；
   2.指定动画的时长；
*/
div{
  animation: myfirst 5s;
  -moz-animation: myfirst 5s;       /* Firefox */
  -webkit-animation: myfirst 5s;    /* Safari 和 Chrome */
  -o-animation: myfirst 5s;         /* Opera */
}
```

**通过百分比设置动画发生的时间** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_animation2)

动画是使元素从一种样式逐渐变化为另一种样式的效果。可以改变任意多的样式任意多的次数。可以用关键词 "from" 和 "to"来设置动画变化发生的时间，其效果等同于 0% 和 100%。0% 是动画的开始，100% 是动画的完成。为了得到最佳的浏览器支持，应该始终定义 0% 和 100% 选择器。

``` css
/* 当动画为 25% 及 50% 时改变背景色，然后当动画 100% 完成时再次改变 */
@keyframes myfirst{
  0%   {background: red;}
  25%  {background: yellow;}
  50%  {background: blue;}
  100% {background: green;}
}

/* 同时改变背景色和位置 */
@keyframes myfirst{
  0%   {background: red; left:0px; top:0px;}
  25%  {background: yellow; left:200px; top:0px;}
  50%  {background: blue; left:200px; top:200px;}
  75%  {background: green; left:0px; top:200px;}
  100% {background: red; left:0px; top:0px;}
}
```

**动画属性详解**

`animation` 是除了 `animation-play-state` 属性所有动画属性的简写属性。

**语法：** animation : animation-name | animation-duration | animation-timing-function | animation-delay | animation-iteration-count | animation-direction

``` css
/* 应用的动画为myfirst，一个动画周期为5秒，动画的速度曲线为linear，动画2秒后播放，播放次数为infinite，即无限循环，动画下一周期是否逆向播放取值alternate，即逆向播放 */
div{
  animation: myfirst 5s linear 2s infinite alternate;
  /* Firefox: */
  -moz-animation: myfirst 5s linear 2s infinite alternate;
  /* Safari 和 Chrome: */
  -webkit-animation: myfirst 5s linear 2s infinite alternate;
  /* Opera: */
  -o-animation: myfirst 5s linear 2s infinite alternate;
}
```

| 属性                        | 描述                                      |
| ------------------------- | --------------------------------------- |
| @keyframes                | 规定动画。                                   |
| animation                 | 所有动画属性的简写属性，除了 animation-play-state 属性。 |
| animation-name            | 规定 @keyframes 动画的名称。                    |
| animation-duration        | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。               |
| animation-timing-function | 规定动画的速度曲线。默认是 "ease"。                   |
| animation-delay           | 规定动画何时开始。默认是 0。                         |
| animation-iteration-count | 规定动画被播放的次数。默认是 1。                       |
| animation-direction       | 规定动画是否在下一周期逆向地播放。默认是 "normal"。          |
| animation-play-state      | 规定动画是否正在运行或暂停。默认是 "running"。            |
| animation-fill-mode       | 规定对象动画时间之外的状态。                          |

### 九、CSS3 多列

通过 CSS3够创建多个列来对文本进行布局，就像我们经常看到的报纸的布局一样。

**CSS3 创建多列**  [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_column-count)

`column-count` 属性规定元素应该被分隔的列数。

``` css
/* 将div中的文本分为3列 */
div{
  column-count:3;
  -moz-column-count:3;        /* Firefox */
  -webkit-column-count:3;     /* Safari 和 Chrome */
}
```

**CSS3 规定列之间的间隔** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_column-gap)

`column-gap` 属性规定列之间的间隔。

``` css
/* 设置列之间的间隔为 40 像素 */
div{
  column-gap:40px;
  -moz-column-gap:40px;        /* Firefox */
  -webkit-column-gap:40px;     /* Safari 和 Chrome */
}
```

**CSS3 列规则** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_column-rule)

`column-rule` 属性设置列之间的宽度、样式和颜色规则。

**语法：** column-rule : column-rule-width | column-rule-style | column-rule-color

```css
div{
  column-rule:3px outset #ff0000;
  -moz-column-rule:3px outset #ff0000;       /* Firefox */
  -webkit-column-rule:3px outset #ff0000;    /* Safari and Chrome */
}
```

| 属性                | 描述                                 |
| ----------------- | ---------------------------------- |
| column-count      | 规定元素应该被分隔的列数。                      |
| column-fill       | 规定如何填充列。                           |
| column-gap        | 规定列之间的间隔。                          |
| column-rule       | 设置所有 column-rule-* 属性的简写属性。        |
| column-rule-width | 规定列之间规则的宽度。                        |
| column-rule-style | 规定列之间规则的样式。                        |
| column-rule-color | 规定列之间规则的颜色。                        |
| column-span       | 规定元素应该横跨的列数。                       |
| column-width      | 规定列的宽度。                            |
| columns           | 语法 : column-width  column-count。 |

### 十、CSS3 用户界面

**CSS3 resize**  [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_resize)

在 CSS3中`resize` 属性设置是否可由用户调整元素尺寸。

``` css
/* 设置div可以由用户调整大小 */
div{
  resize:both;
  overflow:auto;
}
```

**CSS3 box-sizing**  [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_box-sizing)

`box-sizing` 属性允许您以确切的方式定义适应某个区域的具体内容。

**这个属性没太懂**

``` css
/* 规定两个并排的带边框方框 */
div{
  box-sizing:border-box;
  -moz-box-sizing:border-box;        /* Firefox */
  -webkit-box-sizing:border-box;     /* Safari */
  width:50%;
  float:left;
}
```

**CSS3 outline-offset** [示例](http://www.w3school.com.cn/tiy/t.asp?f=css3_outline-offset)

`outline-offset` 属性对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓。

轮廓与边框有两点不同：
> * 轮廓不占用空间；
> * 轮廓可能是非矩形；
``` css
/* 规定边框边缘之外 15 像素处的轮廓 */
div{
  border:2px solid black;
  outline:2px solid red;
  outline-offset:15px;
}
```

### 十一、参考资料

[w3school  CSS3 教程](http://www.w3school.com.cn/css3/)

[CSS3 Border-image](http://www.w3cplus.com/content/css3-border-image)
