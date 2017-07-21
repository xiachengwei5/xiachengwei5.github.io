---
title: Javascript复习
date: 2016-12-25 22:31:18
tags: [js]
---
#### 1.字面量（常量），对象字面量的声明和取值：

``` javascript
   var config = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
   或
   var config = {
     firstName = "John";
     lastName = "Doe";
     age = 50;
     eysColor = "blue";
     fullName: function(){
       return this.fisrtName +" "+ lastName;
     }
   };

   config.firstName == "John";
   config.lastName == "Doe";
   config.age == 50;
   config.eyeColor == "blue";
   config["eyeColor"] == "blue"
   config.fullName() == "John Doe";			//fullName()作为方法调用；
   config.fullName == function(){			//fullName作为属性调用；
       return this.fisrtName +" "+ lastName;
   };
   参考：http://www.runoob.com/js/js-obj-intro.html
```
<!-- more -->
#### 2.数组的声明和取值：

``` javascript
   //直接指定数组内容
   var cars = ["Saab", "Volvo", "BMW"];
   cars[-1] == "undefined"			//不在数字下标范围内的显示undefined
   cars[0] == "Saab"
   cars[1] == "Volvo"
   cars[2] == "BMW"
   cars[3] == "undefined"			//不在数字下标范围内的显示undefined

   //先声明再赋值
   var cars = new Array();
   cars[0] = "Saab";
   cars[1] = "Volvo";
   cars[2] = "BMW";

   //声明时直接赋值
   var cars = new Array("Saab","Volvo","BMW");
```

#### 3.Javascript的数据类型

   > * 字符串（String）、数字(Number)、布尔(Boolean)、数组(Array)、对象(Object)、空（Null）、未定义（Undefined）；
   >
   > * JavaScript 变量均为对象，当声明一个变量时，就创建了一个新的对象；
   >

   JavaScript 拥有动态类型。这意味着相同的变量可用作不同的类型：

``` javascript
   var x;				//声明变量
   x = 5;				//数字类型
   x = "Marl";			//字符串类型
```

   JavaScript 只有一种数字类型。数字可以带小数点，也可以不带：

``` javascript
   var x = 34.00;
   var y = 34;
   console.log(x +"==>"+ y +"==>"+ (x == y));
   结果：34==>34==>true
```

   如果您把值赋给尚未声明的变量，该变量将被自动作为全局变量声明（**即使在函数内执行**），如下：

``` javascript
   car_name = "Volvo";		//直接给尚未声明的变量赋值，会自动作为全局变量声明，即使在函数内执行
```

#### 4.Javascript的作用域

   JavaScript 变量的生存期：

   > - JavaScript 变量的生命期从它们被声明时初始化。
   > - 局部变量会在函数运行以后被销毁。
   > - 全局变量会在页面关闭后被销毁。

   在 HTML 中, 全局变量是 window 对象: 所有数据变量都属于 window 对象

   ​

#### 5.Javascript的运算符

   **赋值运算符**

   =			x = y

   +=			x += y			x = x + y

   -=			x -+ y			x = x - y

   *=			x *= y			x = x * y

   /=			x /= y			x = x / y

   %=		  x %=y			  x = x % y

   **比较运算符**

   1、等于（==）和绝对等于/值和类型都相等（===）

   A.对于string,number等基础类型，==和===是有区别的

   > 1）不同类型间比较，==只比较“转化成同一类型后的值”看“值”是否相等，===如果类型不同，其结果就是不等
``` javascript
   var x = 10;
   var y = "10";
   x == y;        //true;
   x === y;       //false;
```
   >
   > 2）同类型比较，直接进行“值”比较，两者结果一样

   B.对于Array,Object等高级类型，==和===是没有区别的，都是进行“指针地址”比较
``` javascript
   var x = new Object(10);
   var y = new Object("10");
   var z = new Object(10);
   x == y;        //false;
   x === z;       //false;
```

   C.基础类型与高级类型，==和===是有区别的

   > 1）对于==，将高级转化为基础类型，进行“值”的比较
   > 2）因为类型不同，===结果为false
``` javascript
   var x = new Object("10");
   var y = 10;
   x == y;        //true
   x === y;       //false
```

   2、不等于（!=）和不绝对等于/值或类型都不相等（!==）
``` javascript
   var x = 5;
   var y = "5";
   x != y;			//false
   x !== y;		//true
```

#### 6.undefinded和null的区别
``` javascript
   typeof undefined             // undefined
   typeof null                  // object
   null === undefined           // false
   null == undefined            // true
```

#### 7.Javascript类型转换

1、typeof 操作符

> 使用typeof操作符来查看Javascript的数据类型
``` javascript
typeof "John"                 // 返回 string
typeof 3.14                   // 返回 number
typeof NaN                    // 返回 number
typeof false                  // 返回 boolean
typeof [1,2,3,4]              // 返回 object
typeof {name:'John', age:34}  // 返回 object
typeof new Date()             // 返回 object
typeof function () {}         // 返回 function
typeof myCar                  // 返回 undefined (如果 myCar 没有声明)
typeof null                   // 返回 object
```

   2、constructor 属性
> constructor 属性返回所有 JavaScript 变量的构造函数

``` javascript
"John".constructor                 // 返回函数 String()  { [native code] }
(3.14).constructor                 // 返回函数 Number()  { [native code] }
false.constructor                  // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
new Date().constructor             // 返回函数 Date()    { [native code] }
function () {}.constructor         // 返回函数 Function(){ [native code] }
```
> 可以使用 constructor 属性来查看是对象是否为数组 (包含字符串 "Array"):
``` javascript
function isArray(myArray) {
return myArray.constructor.toString().indexOf("Array") > -1;
}
```
3、日期转换为字符串的函数
``` javascript
getDate()			从 Date 对象返回一个月中的某一天 (1 ~ 31)。
getDay()			从 Date 对象返回一周中的某一天 (0 ~ 6)。
getFullYear()		从 Date 对象以四位数字返回年份。
getHours()			返回 Date 对象的小时 (0 ~ 23)。
getMilliseconds()	 返回 Date 对象的毫秒(0 ~ 999)。
getMinutes()		返回 Date 对象的分钟 (0 ~ 59)。
getMonth()			从 Date 对象返回月份 (0 ~ 11)。
getSeconds()		返回 Date 对象的秒数 (0 ~ 59)。
getTime()			返回 1970 年 1 月 1 日至今的毫秒数。
```

4、自动类型转换
> 当 JavaScript 尝试操作一个 "错误" 的数据类型时，会自动转换为 "正确" 的数据类型
``` javascript
5 + null    // 返回 5         null 转换为 0
"5" + null  // 返回"5null"    null 转换为 "null"
"5" + 1     // 返回 "51"      1 转换为 "1"  
"5" - 1     // 返回 4         "5" 转换为 5
```

#### 8.Javascript变量提升(Hoisting)和严格模式（use strict）

> 1、变量提升：函数声明和变量声明总是会被解释器悄悄地被"提升"到方法体的最顶部，但只会提升声明，不会提升初始化；但是这种先使用后声明的写法既不规范也不易于阅读理解所以还是提倡在顶部声明变量；
``` javascript
var x = 5;
var z = x + y;		//NaN，代码执行到此处时y还没有被初始化，但是由于变量提升已经声明
var y = 7;
```
> 2、严格模式：通过在脚本或函数的头部添加 "use strict"; 表达式来声明；
``` javascript
	"use strice";
	x = 3.14;		//报错（x未定义）

	x = 3.14;		//不报错
	"use strice";
	function myFunction(){
		y = 1640;	//报错（y未定义）
	}
```
> 严格模式下的限制：
> * 不允许使用未声明的变量
> * 不允许删除变量或对象
> * 不允许删除函数
> * 不允许变量重名
> * 不允许使用八进制
> * 不允许使用转义字符
> * 不允许对只读属性赋值
> * 不允许对一个使用getter方法读取的属性进行赋值
> * 不允许删除一个不允许删除的属性
> * 变量名不能使用 "eval" 字符串
> * 变量名不能使用 "arguments" 字符串
> * 等......

#### 9.Javascript使用误区

> 1、赋值运算符应用错误
``` javascript
 var x = 0;
 if (x = 10)		//true，并且x的值已经被赋值为10

 var x = 0;
 if (x = 0)		//false，因为javascript中0表示false，1表示true
> 原因：赋值语句返回的是变量的值
```

> 2、比较运算符常见错误
> ​	 A.在常规的比较中，数据类型是被忽略的
``` javascript
 var x = 10;
 var y = "10";
 if (x == y)			//true
```

 ​	B.但在switch 语句会使用恒等计算符(===)进行比较

``` javascript
 var x = 10;
 switch(x) {
     case 10: alert("Hello");		//会执行弹窗
   	case "10": alert("World");		//不会执行弹窗
 }
```

> 3、*浮点型数据使用注意事项*（**这个很容易忽略**）
>
> JavaScript 中的所有数据都是以 64 位浮点型数据(float) 来存储。
>
> 所有的编程语言，包括 JavaScript，对浮点型数据的精确度都很难确定：

``` javascript
 var x = 0.1;
 var y = 0.2;
 var z = x + y            // z 的结果为 0.30000000000000004
 if (z == 0.3)            // 返回 false

> //可以通过下面的方式进行计算
 var z = (x * 10 + y * 10) / 10;       // z 的结果为 0.3，先转换为int再转换为float
```

> 4、Undefined 和 Null混淆
> 在 JavaScript 中, null 用于对象, undefined 用于变量，属性和方法。
> 对象只有被定义才有可能为 null，否则为 undefined。
> 如果我们想测试对象是否存在，在对象还没定义时将会抛出一个错误。
> 错误的使用方式：

``` javascript
 if (myObj !== null && typeof myObj !== "undefined")
```
> 正确的方式是我们需要先使用 typeof 来检测对象是否已定义：

``` javascript
 if (typeof myObj !== "undefined" && myObj !== null)
```

> 5、程序块的作用域
>
> 在每个代码块中 JavaScript 不会创建一个新的作用域，一般各个代码块的作用域都是**全局**的，但是在函数中声明的变量是局部变量。

``` javascript
 //for循环代码块中
 for (var i = 0; i < 10; i++) {
     // some code
 }
 return i;			//i的值为10，而不是undefinded

 //if代码块中
 if(true){
 	var x = 55;
 }
 return x;			//x的值为55，而不是undefinded

 //函数中
 function myFunction(){
 	var x = 33;
 }
 return x;			//x的值为undefinded，因为在函数中声明的变量为局部变量
```


#### 10.Javascript JSON
