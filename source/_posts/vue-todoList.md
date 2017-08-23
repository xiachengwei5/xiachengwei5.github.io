---
title: TodoList小工具——Vue入门实战
data: 2017-08-23 15:33:00
tags: [vue, todoList]
---

用[vue.js](https://cn.vuejs.org/) 编写的一个功能完善的todoList小工具，将Vue的一大核心功能——响应的数据绑定用得淋漓尽致，覆盖了很多Vue的基础知识点，适合作为入门的实战练习。

### 一、覆盖的知识点：

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
...
```

<!-- more -->

### 二、主要实现的功能

> * 添加任务；
> * 删除任务；
> * 本地存储；
> * 筛选任务
> * 统计未完成任务数；

### 三、实现后的效果

![演示效果](http://olywxnzqu.bkt.clouddn.com/img/vue-todoList/todoList.gif) 

### 四、核心代码

CSS:

``` CSS
body {
    margin:0;
    background-color: #fafafa;
    font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

h2{
    margin:0;
    font-size: 12px;
}
a {
    color: #000;
    text-decoration: none;
}

input {
    outline: 0;
}

button {
    margin: 0;
    padding: 0;
    border: 0;
    background: none;
    font-size: 100%;
    vertical-align: baseline;
    font-family: inherit;
    font-weight: inherit;
    color: inherit;
    outline: 0;
}

ul {
    padding:0;
    margin:0;
    list-style: none;
}

.page-top {
    width: 100%;
    height: 40px;
    background-color: #373fdb;
}

.page-content {
    width: 50%;
    margin: 0px auto;
}

.page-content h2{
    line-height: 40px;
    font-size: 18px;
    color: #fff;
}

.main {
    width: 50%;
    margin: 0px auto;
    box-sizing:border-box;
}

.task-input {
    width: 99%;
    height:30px;
    outline: 0;
    border: 1px solid #ccc;
}

.task-count{
    display: flex;
    margin:10px 0;
}

.task-count li {
    padding-left: 10px;
    flex: 1;
    color: #ff0000;
}

.task-count li:nth-child(1){
    padding: 5px 0 0 10px;
}

.action {
    text-align: center;
    display: flex;
}
.action a {
    margin: 0px 10px;
    flex: 1;
    padding: 5px 0;
    color: #777;

}

.action a:nth-child(3){
    margin-right: 0;
}

.active {
    border: 1px solid rgba(225, 0, 0, 0.5);
}

.tasks {
    background-color: #fff;
}
.no-task-tip {
    padding:10px 0 10px 10px;
    display: block;
    border-bottom: 1px solid #ededed;
    color:#777;
}

.big-title {
    color: #222;
}


.todo-list {
    margin: 0;
    padding: 0;
    list-style: none;
}

.todo-list li {
    position: relative;
    font-size: 16px;
    border-bottom: 1px solid #ededed;
}

.todo-list li:hover {
    background-color: #fafafa;
}


.todo-list li.editing {
    border-bottom: none;
    padding: 0;
}

.todo-list li.editing .edit {
    display: block;
    width: 506px;
    padding: 13px 17px 12px 17px;
    margin: 0 0 0 43px;
}

.todo-list li.editing .view {
    display: none;
}

.todo-list li .toggle {
    text-align: center;
    width: 40px;
    /* auto, since non-WebKit browsers doesn't support input styling */
    height: auto;
    position: absolute;
    top: 5px;
    bottom: 0;
    margin: auto 0;
    border: none; /* Mobile Safari */
    -webkit-appearance: none;
    appearance: none;
}

.toggle {
    text-align: center;
    width: 40px;
    /* auto, since non-WebKit browsers doesn't support input styling */
    height: auto;
    position: absolute;
    top: 5px;
    bottom: 0;
    margin: auto 0;
    border: none; /* Mobile Safari */
    -webkit-appearance: none;
    appearance: none;
}

 .toggle:after {
    content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="40" fill="none" stroke="#373fdb" stroke-width="3"/></svg>');
}

.toggle:checked:after {
    content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="40" fill="none" stroke="#bddad5" stroke-width="3"/><path fill="#5dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z"/></svg>');
}

.todo-list li label {
    white-space: pre-line;
    word-break: break-all;
    padding: 15px 60px 15px 15px;
    margin-left: 45px;
    display: block;
    line-height: 1.2;
    transition: color 0.4s;
}

.todo-list li.completed label {
    color: #d9d9d9;
    text-decoration: line-through;
}

/*.tip-toggle {
    padding-left: 0;
    position: relative;
}

.tip-toggle .toggle {
    top: -2px;
}

.tip-toggle span {
    margin-left: 45px;
}*/

.todo-list li .destroy {
    display: none;
    position: absolute;
    top: 5px;
    right: 10px;
    bottom: 0;
    width: 40px;
    height: 40px;
    font-size: 30px;
    color: #ff0000;
    margin-bottom: 11px;
    transition: color 0.2s ease-out;
}

.todo-list li .destroy:hover {
    color: #ff0000;
}

.todo-list li .destroy:after {
    content: '×';
}

.todo-list li:hover .destroy {
    display: block;
}

.todo-list li .edit {
    display: none;
}

.todo-list li.editing:last-child {
    margin-bottom: -1px;
}
```

HTML:

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>To Do List</title>
    <link rel="stylesheet" href="css/index.css">
    <script src="js/vue.js"></script>
</head>
<body>
<div class="page-top">
    <div class="page-content">
        <h2>任务计划列表</h2>
    </div>
</div>
<div class="main">
    <h3 class="big-title">添加任务：</h3>
    <input
            placeholder="提示：按回车键即可添加任务，任务名称不能为空"
            class="task-input"
            type="text"
            v-model="todo"
            @keyup.13="addTodo"
    />
    <ul class="task-count" v-show="list.length">
        <li>{{ unfinishedCount }}个任务未完成</li>
        <li class="action">
            <a :class="{ active: visibility === 'all' }" href="#all">所有任务</a>
            <a :class="{ active: visibility === 'unfinished' }"href="#unfinished">未完成的任务</a>
            <a :class="{ active: visibility === 'finished' }"href="#finished">已完成的任务</a>
        </li>
    </ul>
    <h3 class="big-title">任务列表：</h3>
    <div class="tasks">

        <span class="no-task-tip" v-show="!list.length">还没有添加任何任务</span>
        <ul class="todo-list">
            <li class="todo" :class="{ completed: item.isChecked }" v-for="item in filteredList">
                <div class="view">
                    <input class="toggle" type="checkbox" v-model="item.isChecked"/>
                    <label>{{ item.title }}</label>
                    <button class="destroy" @click="deleteTodo(item)"></button>
                </div>
            </li>
        </ul>
    </div>
</div>
<script src="js/app.js"></script>
</body>
</html>
```

javascript:

``` javascript
/*var list = [
 {
 title: '吃饭',
 isChecked: true         //true表示选中状态(已办任务)
 },{
 title: '睡觉',
 isChecked: false        //false表示未选中状态（待办任务）
 },{
 title: '打豆豆',
 isChecked: false
 }
 ];*/


//本地存储
var store = {
    save(key, value){
        //将数据存到本地缓存中，将获取的对象转换为字符串
        localStorage.setItem(key, JSON.stringify(value));
    },
    fetch(key){
        //从本地缓存中获取缓存数据并转换为Json对象，如果没有则返回空数组
        return JSON.parse(localStorage.getItem(key)) || [];
    }
};

//从本地缓存中获取数据
var list = store.fetch("todoList");

//配置过滤器
var filter = {
    all: function (list) {
        return list;
    },
    unfinished: function (list) {
        return list.filter(function (item) {
            return !item.isChecked;
        })
    },
    finished: function (list) {
        return list.filter(function (item) {
            return item.isChecked;
        })
    }
};


var vm = new Vue({
    el: '.main',
    data: {
        list,               //数据列表
        todo: '',           //记录新增的数据
        visibility: 'all'   //通过控制该属性的值进行筛选
    },
    methods: {
        addTodo(){      //添加任务
            if ("" != this.todo.trim()) {
                //向list中添加任务
                this.list.push({
                    title: this.todo,
                    isChecked: false
                });
                //调用本地缓存的方法将最新的数据存到本地缓存中
                //store.save(this.list);
            } else {
                alert("任务名称不能为空");
            }
            //添加完成后清空文本框中的值
            this.todo = ''
        },
        deleteTodo(item){      //删除任务
            //获取当前项在数组中的索引
            var index = this.list.indexOf(item);
            //根据索引删除当前项，splice()是变异方法，会触发视图更新
            this.list.splice(index, 1);
        }
    },
    watch: {
        //监控list属性，如果对象的个数发生变化时就会调用此函数
        /*list: function () {
         //调用本地缓存的方法将最新的数据存到本地缓存中
         store.save("todoList", this.list);
         }*/

        list: {
            handler: function () {
                store.save("todoList", this.list);
            },
            deep: true          //深度监控：可以发现对象内部值的变化
        }
    },
    computed: {
        //利用计算属性，计算未完成任务的个数
        unfinishedCount: function () {
            return this.list.filter(function (item) {
                return !item.isChecked;
            }).length
        },
        filteredList: function () {
            //如果能匹配上过滤函数就返回过滤后的值，如果没有就返回所有list中的值
            return filter[this.visibility] ? filter[this.visibility](list) : list;
        }
    }
});


function watchHashChage() {
    //获取url中的第一个参数
    var hash = window.location.hash.slice(1);
    //设置过滤函数的参数为hash
    vm.visibility = hash;
}

//注册事件监听器
window.addEventListener("hashchange", watchHashChage);
```

### 五、参考资料

[视频资料](http://pan.baidu.com/s/1hr6uwfy) 密码：7ys1

[项目源码](https://github.com/xiachengwei5/vue-todoList) 
