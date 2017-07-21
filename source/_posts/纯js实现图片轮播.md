---
title: 纯js实现图片轮播
date: 2017-03-05 15:32:04
tags: [js, 图片轮播]
---
用纯js实现图片通过平移的方式向左、向右轮播，基本实现了图片轮播中常用的功能，主要包括：上一张、下一张的切换；任意图片的快速切换；鼠标悬停停止轮播，鼠标离开继续轮播等；
缺点：不能很好的适应不同分辨率的显示器；
[在线预览](/2017/03/05/纯js实现图片轮播/banner/index.html)
[下载](http://olywxnzqu.bkt.clouddn.com/rar/banner/banner.rar)
<!-- more -->
**核心思想：**

通过设置整个图片轴 left 的值来控制图片显示的区域，进而实现图片的平移轮播，模型如下：

![核心思想](http://olywxnzqu.bkt.clouddn.com/img/banner/hexinsixiang.png)

html代码如下：
``` html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>纯js实现图片轮播</title>
		<link rel="stylesheet" type="text/css" href="css/style.css" />
	</head>

	<body>
		<div id="container">
			<ul id="list" style="left: -1920px;">
				<li>
					<a href="#"><img src="img/banner_5.png" width="1920" height="650" alt="" title="" /></a>
				</li>
				<li>
					<a href="#"><img src="img/banner_1.png" width="1920" height="650" alt="" title="" /></a>
				</li>
				<li>
					<a href="#"><img src="img/banner_2.png" width="1920" height="650" alt="" title="" /></a>
				</li>
				<li>
					<a href="#"><img src="img/banner_3.png" width="1920" height="650" alt="" title="" /></a>
				</li>
				<li>
					<a href="#"><img src="img/banner_4.png" width="1920" height="650" alt="" title="" /></a>
				</li>
				<li>
					<a href="#"><img src="img/banner_5.png" width="1920" height="650" alt="" title="" /></a>
				</li>
				<li>
					<a href="#"><img src="img/banner_1.png" width="1920" height="650" alt="" title="" /></a>
				</li>
			</ul>
			<div id="buttons">
				<span index="1" class="on"></span>
				<span index="2"></span>
				<span index="3"></span>
				<span index="4"></span>
				<span index="5"></span>
			</div>
			<a href="javascript:;" id="prev" class="arrow"><img src="img/l.png" /></a>
			<a href="javascript:;" id="next" class="arrow"><img src="img/r.png" /></a>
		</div>
	</body>
	<script src="js/banner.js" type="text/javascript" charset="utf-8"></script>
</html>
```

CSS 代码如下：
``` css
@charset "utf-8";
html,
body {
	min-width: 1280px;
	-webkit-user-select: none;
	-webkit-tap-highlight-color: rgba(0, 0, 0, 0);
	-webkit-tap-highlight-color: transparent;
	-webkit-touch-callout: none;
	-webkit-touch-callout: none;
	-webkit-font-smoothing: antialiased;
	font-family: microsoft yahei, arial;
}

body,
div,
ul,
ol,
li {
	margin: 0;
	padding: 0;
}

ol,
ul {
	list-style: none;
}

#container {
	max-width: 1920px;
	height: 700px;
	overflow: hidden;
	position: relative;
}

#list {
	position: absolute;
	width: 13440px;
	height: 700px;
	z-index: 1;
}

#list img {
	float: left;
}

#buttons {
	position: absolute;
	height: 10px;
	width: 150px;
	z-index: 2;
	bottom: 40px;
	left: 50%;
	margin-left: -36px;
}

#buttons span {
	cursor: pointer;
	float: left;
	border: 2px solid #fff;
	width: 18px;
	height: 18px;
	border-radius: 50%;
	background: #333;
	margin-right: 8px;
}

#buttons .on {
	background: #fff;
}

.arrow {
	cursor: pointer;
	display: none;
	line-height: 39px;
	text-align: center;
	font-size: 36px;
	font-weight: bold;
	width: 40px;
	height: 40px;
	position: absolute;
	z-index: 2;
	top: 50%;
	color: #fff;
}

.arrow:hover {
	background-color: RGBA(255, 255, 255, .7);
}

#container:hover .arrow {
	display: block;
}

#prev {
	left: 20px;
}

#next {
	right: 20px;
}
```

Javascript 代码如下：

``` javascript
window.onload = function() {
	var container = document.getElementById('container');
	var list = document.getElementById('list');
	var buttons = document.getElementById('buttons').getElementsByTagName('span');
	var prev = document.getElementById('prev');
	var next = document.getElementById('next');
	/*定义初始图片的位置*/
	var index = 1;
	/*获取滚动图片的数量*/
	var len = buttons.length;
	var animated = false;
	/*自动轮播时的时间间隔*/
	var interval = 3000;
	var pic_len = 1920;
	/*setTimeout() 返回的 ID 值，用于控制停止动画*/
	var timer;

	/*动画方法，传入“偏移量”作为参数*/
	function animate(offset) {
		if(offset == 0) {
			return;
		}
		animated = true;
		var time = 300;
		var inteval = 10;
		/* 计算每次小的移动的偏移量 */
		var speed = offset / (time / inteval);
		/*每次大的移动完成需要到达的总的偏移量*/
		var left = parseInt(list.style.left) + offset;

		/* 定义函数 */
		var go = function() {
				/* speed>0，也就是offset>0，及图片向右滑动； */
				if((speed > 0 && parseInt(list.style.left) < left) || (speed < 0 && parseInt(list.style.left) > left)) {
					list.style.left = parseInt(list.style.left) + speed + 'px';
					setTimeout(go, inteval);
				} else {
					/* 当向左移动的图片已经移动到最后一张并且还需要向左移动时，先向左移动完完整的一张图片（这就是左边还需要放一张图片的原因），将位置移到初始位置 */
					if(left < (-pic_len * len)) {
						list.style.left = '-' + pic_len + 'px';
					}
					/* 当向右移动的图片已经移动到最后一张并且还需要向右移动时，先向右移动完完整的一张图片（这就是右边还需要放一张图片的原因），将位置移到最后的位置 */
					if(left == 0) {
						list.style.left = -pic_len * len + 'px';
					}
					animated = false;
				}
			}
		/* 调用函数 */
		go();
	}

	/* 切换底部圆形图标“选中”和“非选中”的样式 */
	function showButton() {
		for(var i = 0; i < buttons.length; i++) {
			if(buttons[i].className == 'on') {
				buttons[i].className = '';
				break;
			}
		}
		buttons[index - 1].className = 'on';
	}

	/*播放动画的函数*/
	function play() {
		timer = setTimeout(function() {
			next.onclick();
			play();
		}, interval);
	}

	/*停止动画*/
	function stop() {
		clearTimeout(timer);
	}

	/* 点击右方向图标，图片向右滑动 */
	next.onclick = function() {
		if(animated) {
			return;
		}
		if(index == len) {
			index = 1;
		} else {
			index += 1;
		}
		animate(-pic_len);
		/* 切换底部圆形图标‘选中’和‘非选中’的样式 */
		showButton();
	}

	prev.onclick = function() {
		if(animated) {
			return;
		}
		if(index == 1) {
			index = len;
		} else {
			index -= 1;
		}
		animate(pic_len);
		showButton();
	}

	/* 给下面的每个按钮添加点击事件 */
	for(var i = 0; i < buttons.length; i++) {
		buttons[i].onclick = function() {
			/* 如果动画停止则返回 */
			if(animated) {
				return;
			}
			/* 如果点击的是当前所在的图片则返回 */
			if(this.className == 'on') {
				return;
			}
			var myIndex = parseInt(this.getAttribute('index'));
			/* 计算点击的图片与当前图片之间的偏移量 */
			var offset = -pic_len * (myIndex - index);

			animate(offset);
			index = myIndex;
			showButton();
		}
	}

	/* 鼠标移上时停止动画 */
	container.onmouseover = stop;
	/* 鼠标移出时开始动画 */
	container.onmouseout = play;

	/*js代码初始化完之后调用播放方法*/
	play();
}
```
