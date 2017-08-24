---
title: Vue学习笔记
data: 2017-08-23 15:33:00
tags: [vue, todoList, component]
---

### 2017-08-23 

用`Vue.js` 编写的一个功能完善的`todoList` 小工具，将`Vue` 的一大核心功能——响应的数据绑定用得淋漓尽致，覆盖了很多Vue的基础知识点，适合作为入门的实战练习。

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

学习自定义组件，组件间的通信。

#### 一、覆盖的知识点

``` javascript
component          //自定义组件
props              //子组件要显式地用 props 选项声明它期待获得的数据
$emit              //子组件调用父组件的自定义函数
```

#### 二、实现后的效果

![自定义组件](http://olywxnzqu.bkt.clouddn.com/customComponent.gif) 

### 学习资料

[获取源码](https://github.com/xiachengwei5/vue-todoList) 

[视频资料](http://pan.baidu.com/s/1hr6uwfy) 密码：7ys1

[Vue 官网](https://cn.vuejs.org/) 