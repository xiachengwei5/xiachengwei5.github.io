---
title: Vue学习笔记
date: 2017-08-23 15:33:00
tags: [vue, todoList, component]
---

### 2017-08-23 

用`Vue.js` 编写的一个功能完善的`todoList` 小工具，将`Vue` 的一大核心功能——响应的数据绑定用得淋漓尽致，覆盖了很多Vue的基础知识点，适合作为入门的实战练习。[获取源码](https://github.com/xiachengwei5/vue-study-notes/tree/master/todoList) 

#### 一、覆盖的知识点

``` javascript
v-model              //表单空间绑定（语法糖）
v-bind               //动态绑定属性
v-on                 //绑定事件
v-show               //条件渲染
v-on:keypu.13        //事件修饰符
v-for                //循环
localStorage         //本地存储
filter               //过滤器
watch                //观察模式
computed             //计算属性
directives           //自定义指令
hashchange           //hash过滤
...
```

<!-- more -->

#### 二、主要实现的功能

> * 添加任务；
> * 编辑任务；
> * 删除任务；
> * 筛选任务；
> * 本地存储；
> * 统计未完成任务数；

#### 三、实现后的效果

![演示效果](http://olywxnzqu.bkt.clouddn.com/todoList.gif)  



### 2017-08-24

学习自定义组件，组件间的通信。[获取源码](https://github.com/xiachengwei5/vue-study-notes/tree/master/custom-component) 

#### 一、覆盖的知识点

``` javascript
component          //自定义组件
props              //子组件要显式地用 props 选项声明它期待获得的数据
$emit              //子组件调用父组件的自定义函数
```

#### 二、实现后的效果

![自定义组件](http://olywxnzqu.bkt.clouddn.com/customComponent.gif) 

### 2017-08-21

#### 一、渐进式框架Vue

简单的理解就是Vue的功能很强大很全面，但是你可以根据实际的需求用多少取多少，而不是必须将所有的内容都引入；

![渐进式框架](http://olywxnzqu.bkt.clouddn.com/img/vue-study/jianjinkuangjia.png) 

#### 二、Vue 中的两个核心点 

##### 1、响应的数据绑定 

当数据发生改变时自动更新视图

利用`Object.definedProperty` 中的`setter` / `getter` 代理数据，监控对数据的操作；

##### 2、组合的视图组件

ui 页面映射为组件树

划分组件的优点：**可维护、可重用、可测试** 

#### 三、虚拟DOM

利用在内存中生成与真实DOM与之对应的数据结构，这个在内存中生成的DOM结构称之为虚拟DOM；当数据发生变化时，能够智能的计算出重新渲染组件的最小代价并应用到DOM操作上；真正意义上的局部刷新。



![虚拟DOM](http://olywxnzqu.bkt.clouddn.com/img/vue-study/xunidom1.png) 

![虚拟DOM](http://olywxnzqu.bkt.clouddn.com/img/vue-study/xunidom2.png) 

![虚拟DOM](http://olywxnzqu.bkt.clouddn.com/img/vue-study/xunidom3.png) 

#### 四、MVVM模式

> - M: Model  数据模型
> - V: view   视图模版
> - VM: view-Model   视图模型

![MVVM模式](http://olywxnzqu.bkt.clouddn.com/img/vue-study/MVVM.png)  

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue-Study</title>
    <script type="text/javascript" src="javascript/vue.js"></script>
</head>

<body>
    <!--视图模版-->
    <div id="app">
        {{ message }}
    </div>
</body>

<script type="text/javascript">
    /*数据模型*/
    let obj = {
        message: 'Hello vue.js'
    }

    /*视图模型*/
    var vm = new Vue({
        el: '#app',
        data: obj
    })
</script>
</html>
```

#### 五、声明式渲染

##### 1、声明式渲染

只需要声明在哪里(where)做什么(what)，而**无需关心** 如何实现(how)（重点关注逻辑即可），初始化根实例Vue自动将数据绑定在DOM模版上；

```javascript
//将一个数组的所有值乘以2，等到一个新的数组——声明式渲染
var arr = [1, 2, 3, 4, 5];
var arrArr = arr.map(function (item) {
    return item * 2;
});
```

##### 2、命令式渲染

需要以具体的代码表达在哪里(where)做什么(what)，以及说明如何实现(how)

```javascript
//将一个数组的所有值乘以2，等到一个新的数组——命令式渲染
var arr = [1, 3, 5, 7, 9];
var arrArr = [];
for (var i = 0; i < arr.length; i++) {
    arrArr.push(arr[i] * 2);
}
```

#### 六、指令

##### 1、v-bind 动态绑定数据

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 简写 -->
<a :href="url"></a>
```

##### 2、v-on 绑定一个事件监听器

```html
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 简写 -->
<a @click="doSomething"></a>
```

#### 七、模版



#### 八、组件

使用组件的好处：

> - 提高开发效率；
> - 方便重复使用；
> - 简化调试步骤；
> - 提升整个项目的可维护性；
> - 便于协同开发；



Vue中的组件：

vue中组件是一个自定义标签，Vue.js的编译器为它添加特殊功能，vue也可以扩展原生的html元素，封装可重用的代码；

组件的基本组成：

> - 样式结构；
> - 行为逻辑；
> - 数据；

### 学习资料

[获取源码](https://github.com/xiachengwei5/vue-study-notes) 

[视频资料](http://pan.baidu.com/s/1hr6uwfy) 密码：7ys1

[Vue 官网](https://cn.vuejs.org/) 