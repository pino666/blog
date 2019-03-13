---
title: ES6--generator函数的异步知识
date: 2018-08-22 11:20:33
tags:
    -JavaScript
    -ES6
    -generator
# cover: /assets/twelfth.jpg
---
{% blockquote %}
本文参考于ECMAScript 6 入门--阮一峰 http://es6.ruanyifeng.com/#docs/class
{% endblockquote %}

### 传统方法
ES6 诞生以前，异步编程的方法，大概有下面四种：
<table><tr><td bgcolor=#D1EEEE>1.回调函数
2.事件监听
3.发布/订阅
4.Promise 对象
</td></tr></table>

### 基本概念
#### 异步
所谓"异步"，简单说就是一个任务不是连续完成的，可以理解成该任务被人为分成两段，先执行第一段，然后转而执行其他任务，等做好了准备或者是第一段已经完成，再回过头执行第二段～
相应地，连续的执行就叫做同步；由于是连续执行，不能插入其他任务，所以操作系统从硬盘读取文件的这段时间，程序只能干等着～

#### 回调函数 
回调函数，就是把任务的第二段单独写在一个函数里面，等到重新执行这个任务的时候，就直接调用这个函数～

#### Promise
之前有详细解释，请看之前的博客～

#### Generator 函数
协程：多个线程互相协作，完成异步任务～
<table><tr><td bgcolor=#D1EEEE>运行流程如下：
第一步，协程A开始执行～
第二步，协程A执行到一半，进入暂停，执行权转移到协程B～
第三步，（一段时间后）协程B交还执行权～
第四步，协程A恢复执行～
上面流程的协程A，就是异步任务，因为它分成两段（或多段）执行～
</td></tr></table>那么协程的 Generator 函数是如何实现的？
Generator 函数是协程在 ES6 的实现，最大特点就是可以交出函数的执行权（即暂停执行）～
整个 Generator 函数就是一个封装的异步任务，或者说是异步任务的容器。异步操作需要暂停的地方，都用yield语句注明～
<table><tr><td bgcolor=#D1EEEE>function\* gen(x) {
  var y = yield x + 2;
  return y;
}
var g = gen(1);
g.next() // { value: 3, done: false }
g.next() // { value: undefined, done: true }
解释：
上面代码中，调用 Generator 函数，会返回一个内部指针（即遍历器）g。这是 Generator 函数不同于普通函数的另一个地方，即执行它不会返回结果，返回的是指针对象。调用指针g的next方法，会移动内部指针（即执行异步任务的第一段），指向第一个遇到的yield语句，上例是执行到x + 2为止
</td></tr></table>Generator 函数可以暂停执行和恢复执行，这是它能封装异步任务的根本原因。除此之外，它还有两个特性，使它可以作为异步编程的完整解决方案：函数体内外的数据交换和错误处理机制～<table><tr><td bgcolor=#D1EEEE>next返回值的 value 属性，是 Generator 函数向外输出数据；next方法还可以接受参数，向 Generator 函数体内输入数据～</td></tr></table>Generator 函数内部还可以部署错误处理代码，捕获函数体外抛出的错误～<table><tr><td bgcolor=#D1EEEE>function\* gen(x){
  try {
    var y = yield x + 2;
  } catch (e){
    console.log(e);
  }
  return y;
}
var g = gen(1);
g.next();
g.throw('出错了');
// 出错了
上面代码的最后一行，Generator 函数体外，使用指针对象的throw方法抛出的错误，可以被函数体内的try...catch代码块捕获~
这意味着，出错的代码与处理错误的代码，实现了时间和空间上的分离，这对于异步编程无疑是很重要的~
</td></tr></table>

### 语法糖
#### async 函数
async 函数就是 Generator 函数的语法糖～
实际上，async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await～<table><tr><td bgcolor=#D1EEEE>async函数对 Generator 函数的改进，体现在以下四点：
（1）内置执行器
async函数的执行，与普通函数一模一样，只要一行。
    asyncReadFile();
（2）更好的语义
async和await，比起星号和yield，语义更清楚了；async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果～
（3）更广的适用性
co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时等同于同步操作）～
（4）返回值是 Promise
async函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了，你可以用then方法指定下一步的操作～
进一步说，async函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而await命令就是内部then命令的语法糖～
</td></tr></table>

#### 基本用法
async函数返回一个 Promise 对象，可以使用then方法添加回调函数，当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句～<table><tr><td bgcolor=#D1EEEE>function timeout(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}
async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}
asyncPrint('hello world', 50);
上面代码指定 50 毫秒以后，输出hello world
</td></tr></table>

#### 语法
1.返回 Promise 对象
async函数返回一个 Promise 对象～
async函数内部return语句返回的值，会成为then方法回调函数的参数～<table><tr><td bgcolor=#D1EEEE>async function f() {
  return 'hello world';
}
f().then(v => console.log(v))
// "hello world"
上面代码中，函数f内部return命令返回的值，会被then方法回调函数接收到～
async函数内部抛出错误，会导致返回的 Promise 对象变为reject状态，抛出的错误对象会被catch方法回调函数接收到～
async function f() {
  throw new Error('出错了');
}
f().then(
  v => console.log(v),
  e => console.log(e)
)
// Error: 出错了
</td></tr></table>2.Promise 对象的状态变化<table><tr><td bgcolor=#D1EEEE>async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误～
也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数～</td></tr></table>3.await 命令<table><tr><td bgcolor=#D1EEEE>正常情况下，await命令后面是一个 Promise 对象；如果不是，会被转成一个立即resolve的 Promise 对象～
await命令后面的 Promise 对象如果变为reject状态，则reject的参数会被catch方法的回调函数接收到～
只要一个await语句后面的 Promise 变为reject，那么整个async函数都会中断执行～</td></tr></table>那么即使前一个异步操作失败，也不要中断后面的异步操作的方法是什么呢？<table><tr><td bgcolor=#D1EEEE>1.await放在try...catch结构里面，这样不管这个异步操作是否成功，第二个await都会执行～
async function f() {
  try {
    await Promise.reject('出错了');
  } catch(e) {
  }
  return await Promise.resolve('hello world');
}
f()
.then(v => console.log(v))
// hello world
2.await后面的 Promise 对象再跟一个catch方法，处理前面可能出现的错误～
async function f() {
  await Promise.reject('出错了')
    .catch(e => console.log(e));
  return await Promise.resolve('hello world');
}
f()
.then(v => console.log(v))
// 出错了
// hello world
</td></tr></table>4.错误处理
如果await后面的异步操作出错，那么等同于async函数返回的 Promise 对象被reject～
防止出错的方法，也是将其放在try...catch代码块之中～
如果有多个await命令，可以统一放在try...catch结构中～

#### 使用注意点 
<table><tr><td bgcolor=#D1EEEE>1.最好把await命令放在try...catch代码块中～
2.多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发～
3.await命令只能用在async函数之中，如果用在普通函数，就会报错～
</td></tr></table>