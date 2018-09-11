---
title: ES6常用特性简介
date: 2018-09-11
author: 李赛飞
---
# ES6 常用特性简介

## ECMAScript与JavaScript的关系

ES是JS的标准,JS是ES的一种实现。

## 10个必须了解的特性

1.默认参数
2.模版对象
3.多行字符串
4.解构赋值
5.增强的对象字面量
6.箭头函数
7.Promise
8.块级作用域声明let & const
9.类
10.模块

### Default Parameters

ES5定义默认参数的方式:

```javascript
var link = function (height, color, url) {
    var height = height || 50
    var color = color || 'red'
    var url = url || 'http://baidu.com'
    ...
}
```

这种写法在参数值是0时，存在缺陷，因为在JS中，0表示falsy,默认被hard-coded的值，无法变成参数本身的值。例子：

```javascript
function test(a, b) {
    var a = a || 10;
    var b = b || 20;
    console.log(a);
    console.log(b);
}
test(0, 1);     // Output: 10 1
```

ES6写法：

```javascript
var link = function(height = 50, color = 'red', url = 'https://google.com') {
  ...
}
```

### Template Literals

ES5组合字符串的方式:

```javascript
var name = 'Your name is ' + first + ' ' + last + '.';
var url = 'http://localhost:3000/api/messages/' + id;
```

ES6语法： `${NAME}` + `反引号`

```javascript
var name = `Your name is ${first} ${last}. `;
var url = `http://localhost:3000/api/messages/${id}`;
```

### Multi-line Strings

ES5表示多行字符串:

```javascript
var roadPoem = 'Then took the other, as just as fair,\n\t'
    + 'And having perhaps the better claim\n\t'
    + 'Because it was grassy and wanted wear,\n\t'
    + 'Though as for that the passing there\n\t'
    + 'Had worn them really about the same,\n\t'

var fourAgreements = 'You have the right to be you.\n\
    You can only be you when you do your best.'
```

ES6语法：
`反引号` ``

```javascript
var roadPoem = `Then took the other, as just as fair,
    And having perhaps the better claim
    Because it was grassy and wanted wear,
    Though as for that the passing there
    Had worn them really about the same,`

var fourAgreements = `You have the right to be you.
    You can only be you when you do your best.`
```

### 解构赋值

#### 数组的解构赋值

```javascript
let [a, b, c] = [1, 2, 3];
```

只要等号两边的模式相同，左边的变量就会被赋予对应的值。

```javascript
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"
```

如果解析不成功，变量的值为`undefined`。

#### 对象的解构赋值

```javascript
let { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
```

对象的解构与数组有一个重要不同：数组的元素是按次序排列的，**变量的取值由它的位置决定**;而对象的属性没有次序，**变量必须与属性同名**，才能取到正确的值。

```javascript
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```

### Enhanced Object Literals

#### 属性的简洁表示法

ES6 允许直接写入变量和函数，作为对象的属性和方法。

```javascript
const foo = 'bar';
const baz = {foo};
baz // {foo: "bar"}

// 等同于
const baz = {foo: foo};
```

上面的代码表明，ES6 允许在对象之中，直接写变量。这时，属性名为变量名, 属性值为变量的值。下面是另一个例子。

```javascript
function f(x, y) {
  return {x, y};
}

// 等同于

function f(x, y) {
  return {x: x, y: y};
}

f(1, 2) // Object {x: 1, y: 2}
```

方法也可以简写：

```javascript
let ms = {};

function getItem (key) {
  return key in ms ? ms[key] : null;
}

function setItem (key, value) {
  ms[key] = value;
}

function clear () {
  ms = {};
}

module.exports = { getItem, setItem, clear };
// 等同于
module.exports = {
  getItem: getItem,
  setItem: setItem,
  clear: clear
};
```

#### 属性名表达式

ES5 定义对象属性有两种方法：

```javascript
// 方法一
obj.foo = true;

// 方法二
obj['a' + 'bc'] = 123;
```

上面代码的方法一是直接用标识符作为属性名，方法二是用表达式作为属性名，这时要将表达式放在方括号之内。

但是，如果使用字面量方式定义对象（使用大括号），在 ES5 中只能使用方法一（标识符）定义属性。

```javascript
var obj = {
  foo: true,
  abc: 123
};
```

表达式还可以用于定义`方法名`：

```javascript
let obj = {
  ['h' + 'ello']() {
    return 'hi';
  }
};

obj.hello() // hi
```

#### Object.assign()

常用于：
(1).为对象添加属性
(2).为对象添加方法
(3).克隆对象(浅拷贝)
(4).合并多个对象
(5).为属性指定默认值

#### Object.is()

ES5 比较两个值是否相等只有两个运算符： 相等运算符(`==`)和严格相等运算符(`===`).它们存在缺点：

- `==`会自动转换数据类型；
- `===`的NaN不等于自身，以及+0等于-0.

```javascript
+0 === -0 //true
NaN === NaN // false

Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
```

### Arrow Functions

箭头函数表达式比一般函数表达式更简洁，没有自己的`this`,`arguments`,`super`,'new.target'。适用于匿名函数，不能用作构造函数。

基本语法：

```javascript
(param1, param2, …, paramN) => { statements }
(param1, param2, …, paramN) => expression
// equivalent to: => { return expression; }

// Parentheses are optional when there's only one parameter name:
(singleParam) => { statements }
singleParam => { statements }

// The parameter list for a function with no parameters should be written with a pair of parentheses.
() => { statements }
```

#### 更简洁的函数

```javascript
var elements = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];
// ES5
elements.map(function(element ) {
  return element.length;
}); // [8, 6, 7, 9]
// ES6 Arrow Function
elements.map(element => {
  return element.length;
}); // [8, 6, 7, 9]

elements.map(element => element.length); // [8, 6, 7, 9]
```

#### 没有自己的`this`

面向对象编程常常能见到这样的写法：

```javascript
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    //  回调引用的是`that`变量, 其值是预期的对象.
    that.age++;
  }, 1000);
}
```

箭头函数不会创建自己的`this`，它`只会从自己的作用域链的上一层继承this`。因此，在下面的代码中，传递给`setInterval`的函数内的`this`与封闭函数中的`this`相同。

```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| 正确地指向person 对象
  }, 1000);
}

var p = new Person();
```

### Promise

Promise 是异步编程的一种解决方案，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。
基本用法：

```javascript
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

Promise对象有3种状态：

- `pending`
- `fulfilled`
- `rejected`

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

`resolve`函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 `pending` 变为 `resolved`），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；`reject`函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 `pending` 变为 `rejected`），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

我们用`Promise.then()`方法处理Promise的结果。

```javascript
promise.then(onResolve, onReject)
```

#### 回调地狱

假设我们要完成下列事情：
1.运行一段代码
2.等一秒
3.再运行一段代码
4.等一秒
5.再运行一段代码

使用`setTimeout`来实现的话：

```javascript
runAnimation(0);
setTimeout(function() {
    runAnimation(1);
    setTimeout(function() {
        runAnimation(2);
    }, 1000);
}, 1000);
```

看起来很糟糕，对吧？试想一下如果我们要执行10步而不是3步...这就是`回调地狱`.

#### 使用Promise来避免回调地狱

```javascript
function delay(interval) {
    return new Promise(function (resolve) {
        setTimeout(resolve, interval);
    });
}

runAnimation(0);
delay(1000)
    .then(function() {
        runAnimation(1);
        return delay(1000);
    })
    .then(function() {
        runAnimation(2);
    });
```

#### Difference between callbacks and Promise

1.Callbacks是函数，Promise是对象。
2.Callbacks作为参数被传递，而Promise被返回。
3.Callbacks处理成功和失败，Promise不处理任何事。
4.Callbacks可被多次调用，Promise只能代表一个事件。

### Block-Scoped Constructs Let and Const

我们使用花括号定义代码块，在ES5中，`var`声明的变量都是全局的，块级作用域没任何作用。`let` 的作用和 `var` 相似，不同在于`let`把变量的作用域限定在块级。

```javascript
function calculateTotalAmount (vip) {
  var amount = 0
  if (vip) {
    var amount = 1
  }
  { // more crazy blocks!
    var amount = 100
    {
      var amount = 1000
      }
  }  
  return amount
}

console.log(calculateTotalAmount(true)) // 输出：1000
```

使用`let`

```javascript
function calculateTotalAmount (vip) {
  var amount = 0 // probably should also be let, but you can mix var and let
  if (vip) {
    let amount = 1 // first amount is still 0
  }
  { // more crazy blocks!
    let amount = 100 // first amount is still 0
    {
      let amount = 1000 // first amount is still 0
      }
  }  
  return amount
}

console.log(calculateTotalAmount(true))
```

### Class

JavaScript语言中,生成实例对象的传统方法是通过构造函数。例如：

```javascript
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。不过需要知道的是，ES6`class`依然用的是原型模式，不是函数工厂的方式。

基本上，ES6 的class可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。上面的代码用 ES6 的class改写，就是下面这样:

```javascript
//定义类
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

### Modules

在ES6之前，JavaScript没有原生的模块支持方案。社区提出的模块化方案主要分两种：AMD和CommonJS。这两种方案各有长短，有自己适用的场景。主要不同是AMD偏向浏览器环境；CommonJS偏向Node.ES6 modules把这两者的优势结合在了一起：

- 语法简单，类似CommonJS,一个文件一个模块
- 异步模块加载，同AMD。

关键字： `export`和`import`

> **参考资料**
> [阮一峰-ES6入门](http://es6.ruanyifeng.com/)
> [Introduction to ES6 Promise](http://jamesknelson.com/grokking-es6-promises-the-four-functions-you-need-to-avoid-callback-hell/)
> [Top 10 ES6 Features Every Busy JavaScript Developer Must Know](https://webapplog.com/es6/comment-page-1/)
> [MDN](https://developer.mozilla.org/zh-CN/)