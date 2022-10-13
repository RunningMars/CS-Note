JS

## JS的特点

–解释型语言

–类似于 C 和 Java 的语法结构

–动态语言

–基于原型的面向对象的特点

## 数据类型

### JavaScript中一共有5种基本数据类型：

– 字符串型（String）

– 数值型（Number）

– 布尔型（Boolean）

– null型（Null）

Null 类型是第二个只有一个值的数据类型，这个特殊的值是null 。

• 从语义上看null表示的是一个空的对象。所以使用typeof检查null会返回一个Object。

• undefined值实际上是由null值衍生出来的，所以如果比较undefined和null是否相等，会返回true；

– undefined型（Undefined）

Undefined 类型只有一个值，即特殊的 undefined 。

• 在使用 var 声明变量但未对其加以初始化时，这个变量的值就是 undefined。

例如：

var message;

message 的值就是 undefined。

• 需要注意的是typeof对没有初始化和没有声明的变量都会返回undefined。

string标识一个字符串，需要用 ’ 或 “ 括起来

JS中的变量可能包含两种不同数据类型的值：基本数据类型和引用数据类型。

• JS中一共有5种基本数据类型：String、Number、Boolean、Undefined、Null。

引用数据类型 - Object对象

## 数组

• 数组也是对象的一种。

• 数组是一种用于表达有顺序关系的值的集合的语言结构。

• 创建数组：

– var array = [1,44,33];

• 数组内的各个值被称作元素。每一个元素

都可以通过索引（下标）来快速读取。索

引是从零开始的整数。

## 传递参数

• JS中的所有的参数传递都是按值传递的。也就是说把函数外部的值赋值给函数内部的参数，就和把值从一个变量赋值给另一个变量是一样的。

函数内部属性

• 在函数内部，有两个特殊的对象：

### – arguments

• 该对象实际上是一个数组，用于保存函数的参数。

• 同时该对象还有一个属性callee来表示当前函数。

### – this

• this 引用的是一个对象。对于最外层代码与函数内部的情况，其引用目标是不同的。

• 此外，即使在函数内部，根据函数调用方式的不同，引用对象也会有所不同。需要注意的是，this 引用会根据代码的上下文语境自动改变其引用对象。

## new关键字

• 使用new关键字执行一个构造函数时：

– 首先，会先创建一个空的对象。

– 然后，会执行相应的构造函数。构造函数中的this将会引用这个新对象。

– 最后，将对象作为执行结果返回。

• 构造函数总是由new关键字调用。

• 构造函数和普通函数的区别就在于调用方式的不同。

• 任何函数都可以通过new来调用，所以函数都可以是构造函数。

• 在开发中，通常会区分用于执行的函数和构造函数。

• 构造函数的首字母要大写。

__proto__  每个对象都会有一个属性 __proto__，指向原型对象

节点：Node——构成HTML文档最基本的单元。

• 常用节点分为四类

– **文档节点**：整个HTML文档

– **元素节点**：HTML文档中的HTML标签

– **属性节点**：元素的属性

– **文本节点**：HTML标签中的文本内容

 

## 获取元素节点

• 通过document对象调用

\1. getElementById()

– 通过**id**属性获取**一个**元素节点对象

\2. getElementsByTagName()

– 通过**标签名**获取**一组**元素节点对象

\3. getElementsByName()

– 通过**name**属性获取**一组**元素节点对象

事件处理程序

• 我们可以通过两种方式为一个元素绑定事件处理程序：

– 通过HTML元素指定事件属性来绑定

```js
<button onclick="alert('hello');alert('world')">按钮</button>
```

– 通过DOM对象指定的属性来绑定

```js
var btn = document.getElementById('btn');

btn.onclick = function(){

   alert("hello");

};
```



• 这两种方式都是我们日常用的比较多的，但是更推荐使用第二种方式。

• 还有一种方式比较特殊我们称为设置事件监听器。使用如下方式：

– 元素对象.addEventListener()

## 事件对象

• 在DOM对象上的某个事件被触发时，会产生一个事件对象Event，这个对象中包含着所有事件有关的信息。

包括导致事件的元素、事件的类型以及其他与特定事件相关的信息。

• 例如，鼠标操作导致的事件对象中，会包含鼠标位置的信息，而键盘操作导致的事件对象中，会包含

与按下的键有关的信息。所有浏览器都支持 event对象，但支持方式不同。

### IE中的事件对象

• 与访问 DOM 中的 event 对象不同，要访问 IE 中的 event 对象有几种不同的方式，取决于指定事件处理程序的方法。

• 在IE中event对象作为window对象的属性存在的，可以使用window.event来获取event对象。

• 在使用attachEvent()的情况下，也会在处理程序中传递一个event对象，也可以按照前边的方式使用。

### 事件的触发

• 事件的发生主要是由用户操作引起的。

• 比如mousemove这个事件就是由于用户移动鼠标引起的，在鼠标指针移动的过程中该事件会持续发生。

• 当指定事件被触发时，浏览器就会调用对应的函数去响应事件，一般情况下事件没触发一次，函数就会执行一次。

• 因此设置鼠标移动的事件可能会影响到鼠标的移动速度。所以设置该类事件时一定要谨慎。

### 事件的传播

• 捕获阶段

– 这一阶段会从window对象开始向下一直遍历到目标对象，如果发现有对象绑定了响应事件则做相应的处理。

• 目标阶段

– 这一阶段已经遍历结束，则会执行目标对象上绑定的响应函数。

• 事件冒泡阶段

– 这一阶段，事件的传播方式和捕获阶段正好相反，会从事件目标一直向上遍历，直至window对象结束，这时对象上绑定的响应函数也会执行。

## window对象

• window对象是BOM的核心，它表示一个浏览器的实例。

• 在浏览器中我们可以通过window对象来访问操作浏览器，同时window也是作为全局对象存在的。

• 全局作用域：

– window对象是浏览器中的全局对象，因此所有在全局作用域中声明的变量、对象、函数都会变成window对象的属性和方法。

 

## 打开窗口

• 使用 window.open() 方法既可以导航到一个特定的 URL，也可以打开一个新的浏览器窗口。

• 这个方法需要四个参数：

– 需要加载的url地址

– 窗口的目标

– 一个特性的字符串

– 是否创建新的历史记录

 

## 超时调用

• 超时调用：

– setTimeout()

– 超过一定时间以后执行指定函数

– 需要连个参数：

• 要执行的内容

• 超过的时间

• 取消超时调用

– clearTimeout()

• 超时调用都是在全局作用域中执行的。

 

## 间歇调用

• 间歇调用：

– setInterval()

– 每隔一段时间执行指定代码

– 需要两个参数：

• 要执行的代码

• 间隔的时间

• 取消间隔调用：

– clearInterval()

 

## 系统对话框

• 浏览器通过 alert() 、 confirm() 和 prompt()方法可以调用系统对话框向用户显示消息。

• 它们的外观由操作系统及（或）浏览器设置决定，而不是由 CSS 决定。

• 显示系统对话框时会导致程序终止，当关闭对话框程序会恢复执行。

 

## location对象

• location对象提供了与当前窗口中加载的文档有关的信息，还提供了一些导航功能。

• href属性：

– href属性可以获取或修改当前页面的完整的URL地址，使浏览器跳转到指定页面。

• assign() 方法

– 所用和href一样，使浏览器跳转页面，新地址错误参数传递到assign ()方法中

• replace()方法

– 功能一样，只不过使用replace方法跳转地址不会体现到历史记录中。

• reload() 方法

– 用于强制刷新当前页面

 

## navigator对象

• navigator 对象包含了浏览器的版本、浏览器所支持的插件、浏览器所使用的语言等各种与浏览器相关的信息。

• 我们有时会使用navigator的userAgent属性来检查用户浏览器的版本。

history对象• history 对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。

• go()

– 使用 go() 方法可以在用户的历史记录中任意跳转，可以向后也可以向前。

• back()

– 向后跳转

• forward()

– 向前跳转

## 事件冒泡：

 所谓的冒泡指的是事件的向上传导，当后代元素的时间被出发是，其祖先原色的相同事件也会被触发

基本语法和标识符：

语句结尾以；结尾，如果没有分号，浏览器会自动为每一行添加

变量的声明：

var ，

## var let const 的区别

函数：

函数的内部可以声明函数

函数可以返回任何类型的值

当函数调用call() 和 apply()都会调用函数执行

函数与对象的关系 



## json

JavaScript Object Notation ，JS的对象表示法

require和import的区别

数组解构赋值：

```js
let arr = [1,4,7]

let [ a,b,c] = arr
```



对象解构赋值，花括号取值：

```javascript
const {name,pwd} = ctx.request.body

let person = { name:’rdg’,sex:”male”}
```

方式一：

```js
let {name,sex} = person
```

方式二：

```js
let  {name: userName, sex: userSex} = person
```

name赋值给userName变量

## 箭头函数

[es6](https://so.csdn.net/so/search?q=es6&spm=1001.2101.3001.7020) 新增了使用胖箭头（=>）语法定义函数表达式的能力，很大程度上，箭头函数实例化的函数对象与正式的函数表达式创建的函数对象行为是相同的。任何可以使用函数表达式的地方，都可以使用箭头函数：

// 普通函数

```js
let sum = function(a, b) {
 		return a + b;
}
```

// 箭头函数

```js
let sum1 = (a, b) => {
     return a + b;
}
```

// 有效

```js
let sum = (a, b) => {
  	return a + b;
};
```

// 有效

```js
let sum1 = (a, b) => a + b; // 相当于 return a + b;
```

// 无效的写法

```js
let sum2 = (a, b) => return a + b;
```

## Set 是数据结构

类似java的Set，没有重复的值

遍历Set
```js
var set5 = new Set([“a”,”c”]）

set5.forEach()
```





## Import  和 require的使用



Koa2框架中: 请求参数中取user字段, 申明变量userId,并复制给userId;  并没有声明user变量,仅仅是字段key

```nodejs
const all = async (ctx) => {
      let {
        sz,
        p,
        name,
        eth,
        fg,
        meal,
        place,
        minCal,
        maxCal,
        user: userId,
        external,
      } = ctx.request.query;
  }
```





新建对象测试

```js
const app =  "weight_loss";
  let userId = "sdfsdf";
  let timezone = "asia";
  let avatar = "234sdf";

  console.log("dd",{
    pic: avatar,
    app,
    timezone,
    oauth: {
      google: { id: userId },
    },
  });
##########################
  {
      pic: '234sdf',
      app: 'weight_loss',
      timezone: 'asia',
      oauth: { google: { id: 'sdfsdf' } }
  }
```

