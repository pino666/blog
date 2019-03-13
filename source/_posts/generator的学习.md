---
title: ES6-generator函数的基础知识
date: 2018-08-21 18:20:24
tags:
    -JavaScript
    -ES6
    -generator
cover: /assets/nine.jpg
---
{% blockquote %}
本文参考于ECMAScript 6 入门--阮一峰 http://es6.ruanyifeng.com/#docs/class
{% endblockquote %}

## Generator函数的基础介绍
### 基本概念
形式上，Generator函数是一个普通函数，但是有两个特征。1.function关键字与函数名之间有一个星号(*)；2.函数体内部使用yield表达式，定义不同的内部状态
<table><tr><td bgcolor=#D1EEEE>🌰：function\* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}
var hw = helloWorldGenerator();
上面代码定义了一个 Generator函数helloWorldGenerator，它内部有两个yield表达式（hello和world），即该函数有三个状态：hello，world 和 return 语句（结束执行）~
</td></tr></table>Generator 函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。但不同的是，调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，你必须调用遍历器对象的<font color="#dd0000">next</font>方法，使得指针移向下一个状态~
也就是说，每次调用next方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield表达式（或return语句）为止。换言之，Generator 函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行~
<table><tr><td bgcolor=#D1EEEE>🌰：调用上面创建的函数：
hw.next()// { value: 'hello', done: false }
hw.next()// { value: 'world', done: false }
hw.next()// { value: 'ending', done: true }
hw.next()// { value: undefined, done: true }
解释：
前两次调用：Generator 函数都是从上次yield表达式停下的地方，一直执行到下一个yield表达式。next方法返回的对象的value属性就是当前yield表达式的值，done属性的值false，表示遍历还没有结束～
第三次调用：Generator 函数从上次yield表达式停下的地方，一直执行到return语句（如果没有return语句，就执行到函数结束；next方法返回的对象的value属性，就是紧跟在return语句后面的表达式的值（如果没有return语句，则value属性的值为undefined），done属性的值true，表示遍历已经结束～
第四次调用：此时 Generator 函数已经运行完毕，next方法返回对象的value属性为undefined，done属性为true。以后再调用next方法，返回的都是这个值～
</td></tr></table>总结：调用 Generator 函数，会返回一个遍历器对象，代表 Generator 函数的内部指针；以后，每次调用遍历器对象的next方法，就会返回一个有着value和done两个属性的对象。value属性表示当前的内部状态的值，是yield表达式后面那个表达式的值；done属性是一个布尔值，表示是否遍历结束～
<table><tr><td bgcolor=#D1EEEE><font color="#dd0000">注：</font>1.yield表达式只能用在 Generator 函数里面，用在其他地方都会报错~
2.yield表达式如果用在另一个表达式之中，必须放在圆括号里面~
3.yield表达式用作函数参数或放在赋值表达式的右边，可以不加括号~</td></tr></table>

<table><tr><td bgcolor=#D1EEEE>之前我们知道，任意一个对象的Symbol.iterator方法，等于该对象的遍历器生成函数，调用该函数会返回该对象的一个遍历器对象，由于 Generator 函数就是遍历器生成函数，因此可以把 Generator 赋值给对象的Symbol.iterator属性，从而可使得该对象具有 Iterator 接口，这样也就可以被...运算符遍历了～</td></tr></table>

### next方法参数
next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值～
<table><tr><td bgcolor=#D1EEEE>🌰：function\* foo(x) {
  var y = 2 \* (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}
var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}
var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
</td></tr></table>

### for of循环
for...of循环可以自动遍历 Generator 函数时生成的Iterator对象，且此时不再需要调用next方法～
<table><tr><td bgcolor=#D1EEEE>🌰：function\* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}
for (let v of foo()) {
  console.log(v);
}   // 1 2 3 4 5
注：一旦next方法的返回对象的done属性为true，for...of循环就会中止，且不包含该返回对象，所以上面代码的return语句返回的6，不包括在for...of循环之中
</td></tr></table>除了for...of循环以外，扩展运算符（...）、解构赋值和Array.from方法内部调用的，都是遍历器接口。这意味着，它们都可以将 Generator 函数返回的 Iterator 对象，作为参数~

### Generator.prototype.throw()
Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获~
<table><tr><td bgcolor=#D1EEEE>🌰：var g = function\* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};
var i = g();
i.next();
try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b
解释：
上面代码中，遍历器对象i连续抛出两个错误。第一个错误被 Generator 函数体内的catch语句捕获。i第二次抛出错误，由于 Generator 函数内部的catch语句已经执行过了，不会再捕捉到这个错误了，所以这个错误就被抛出了 Generator 函数体，被函数体外的catch语句捕获～
<font color="#dd0000">注：</font>1.如果 Generator 函数内部没有部署try...catch代码块，那么throw方法抛出的错误，将被外部try...catch代码块捕获～
2.throw方法抛出的错误要被内部捕获，前提是必须至少执行过一次next方法～
3.throw命令与g.throw方法是无关的，两者互不影响～
</td></tr></table>

### Generator.prototype.return()
return方法：可以返回给定的值，并且终结遍历 Generator 函数～
<table><tr><td bgcolor=#D1EEEE>🌰：function\* gen() {
  yield 1;
  yield 2;
  yield 3;
}
var g = gen();
g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }
解释：
上面代码中，遍历器对象g调用return方法后，返回值的value属性就是return方法的参数foo。并且，Generator 函数的遍历就终止了，返回值的done属性为true，以后再调用next方法，done属性总是返回true～
如果return方法调用时，不提供参数，则返回值的value属性为undefined～
如果 Generator 函数内部有try...finally代码块，那么return方法会推迟到finally代码块执行完再执行～
</td></tr></table>

### next()、throw()、return() 的共同点
next()、throw()、return()这三个方法本质上是同一件事，可以放在一起理解；它们的作用都是让 Generator 函数恢复执行，并且使用不同的语句替换yield表达式～
<table><tr><td bgcolor=#D1EEEE>🌰：next()是将yield表达式替换成一个值～
throw()是将yield表达式替换成一个throw语句
return()是将yield表达式替换成一个return语句
</td></tr></table>

### yield* 表达式 
如果在 Generator 函数内部，调用另一个 Generator 函数，默认情况下是没有效果的～
<table><tr><td bgcolor=#D1EEEE>🌰：function\* foo() {
  yield 'a';
  yield 'b';
}
function\* bar() {
  yield 'x';
  foo();
  yield 'y';
}
for (let v of bar()){
  console.log(v);
}
// "x"
// "y"
上面代码中，foo和bar都是 Generator 函数，在bar里面调用foo，是不会有效果的～
这个就需要用到yield\*表达式--用来在一个 Generator 函数里面执行另一个 Generator 函数～
function\* bar() {
  yield 'x';
  yield\* foo();
  yield 'y';
}
// 等同于
function\* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}
</td></tr></table>实际上，任何数据结构只要有 Iterator 接口，就可以被yield\*遍历~
<table><tr><td bgcolor=#D1EEEE>🌰：let read = (function\* () {
  yield 'hello';
  yield\* 'hello';
})();

read.next().value // "hello"
read.next().value // "h"
</td></tr></table>

### Generator 函数的this 
Generator 函数总是返回一个遍历器，ES6 规定这个遍历器是 Generator 函数的实例，也继承了 Generator 函数的prototype对象上的方法~
<table><tr><td bgcolor=#D1EEEE>🌰：function\* g() {
  this.a = 11;
}

let obj = g();
obj.next();
obj.a // undefined
解释：
上面代码中，Generator 函数g在this对象上面添加了一个属性a，但是obj对象拿不到这个属性～
Generator 函数也不能跟new命令一起用，会报错～
</td></tr></table>
那么，怎样可以让 Generator 函数返回一个正常的对象实例，既可以用next方法，又可以获得正常的this呢？
变通方法：首先，生成一个空对象，使用call方法绑定 Generator 函数内部的this。这样，构造函数调用以后，这个空对象就是 Generator 函数的实例对象了～
<table><tr><td bgcolor=#D1EEEE>🌰：function\* F() {
  this.a = 1;
  yield this.b = 2;
  yield this.c = 3;
}
var obj = {};
var f = F.call(obj);
f.next();  // Object {value: 2, done: false}
f.next();  // Object {value: 3, done: false}
f.next();  // Object {value: undefined, done: true}
obj.a // 1
obj.b // 2
obj.c // 3
解释：
上面代码中，首先是F内部的this对象绑定obj对象，然后调用它，返回一个 Iterator 对象。这个对象执行三次next方法（因为F内部有两个yield表达式），完成 F 内部所有代码的运行。这时，所有内部属性都绑定在obj对象上了，因此obj对象也就成了F的实例～
但是。上面代码中，执行的是遍历器对象f，但是生成的对象实例是obj，有没有办法将这两个对象统一呢？
将obj换成F.prototype，再将F改成构造函数，就可以对它执行new命令了～
如下更改：
function* gen() {
  this.a = 1;
  yield this.b = 2;
  yield this.c = 3;
}
function F() {
  return gen.call(gen.prototype);
}
var f = new F();
f.next();  // Object {value: 2, done: false}
f.next();  // Object {value: 3, done: false}
f.next();  // Object {value: undefined, done: true}

f.a // 1
f.b // 2
f.c // 3
</td></tr></table>

### 应用
Generator 可以暂停函数执行，返回任意表达式的值。这种特点使得 Generator 有多种应用场景～
    1.异步操作的同步化表达～
    2.控制流管理～
    3.部署 Iterator 接口～
    4.作为数据结构