---
title: ES6--promise异步
date: 2018-08-17 12:27:29
tags:
    - JavaScript
    - Promise
cover: /assets/eleven.jpg
---
{% blockquote %}
本文参考来源于 网络 以及 师傅传授
{% endblockquote %}

## ES6  Promise介绍
Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。
所谓Promise，简单说就是一个容器，里面保存着<font color="#dd0000">某个未来才会结束的事件</font>（通常是一个异步操作）的结果。
从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。
注：promise会在时间循环的末尾执行，晚于本轮循环的同步任务。

### Promise 特点与缺点
<table><tr><td bgcolor=#D1EEEE>1.对象的状态不受外界影响。
Promise对象代表一个异步操作，有三种状态：<font color="#dd0000">pending（进行中）、</font><font color="#dd0000">fulfilled（已成功）</font>和<font color="#dd0000">rejected（已失败）。</font>只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。
2.一旦状态改变，就不会再变，任何时候都可以得到这个结果。
Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。
只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。<font color="#dd0000">这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。</font>
缺点：
1.无法取消Promise，一旦新建它就会立即执行，无法中途取消。
2.如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
3.当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
</td></tr></table>

### Promise基本用法
ES6 规定，Promise对象是一个构造函数，用来生成Promise实例。
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。
<table><tr><td bgcolor=#D1EEEE>resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去。
reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
</td></tr></table>
<table><tr><td bgcolor=#D1EEEE>Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
then方法可以接受两个回调函数作为参数。
第一个回调函数是Promise对象的状态变为resolved时调用，第二个回调函数是Promise对象的状态变为rejected时调用。
其中，第二个函数是可选的，不一定要提供。这两个函数都接受Promise对象传出的值作为参数。
</td></tr></table>


### Promise.prototype.then()
then方法是定义在原型对象Promise.prototype上的。
它的作用是为 Promise 实例添加状态改变时的回调函数。
前面说过，then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数。
then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。
<table><tr><td bgcolor=#D1EEEE>🌰：getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("resolved: ", comments),
  err => console.log("rejected: ", err)
);
</td></tr></table>

### Promise.prototype.catch()[推荐使用]
Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。
<table><tr><td bgcolor=#D1EEEE>🌰：getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
注：如果 Promise 状态已经变成resolved，再抛出错误是无效的。
</td></tr></table>

### Promise.prototype.finally()
finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。
<table><tr><td bgcolor=#D1EEEE>🌰：promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
上面代码中，不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数。
</td></tr></table>

### Promise.all() 
Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
<table><tr><td bgcolor=#D1EEEE>🌰：const p = Promise.all([p1, p2, p3]);
p的状态由p1、p2、p3决定，分成两种情况。
（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。
（注 ：Promise.all方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。)
</td></tr></table>

### Promise.race() 
Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。
但是与all不同的是：只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

### 回调函数 缺点
1.外观--代码风格杂乱
2.特定情况异步，其他情况同步的时候，执行顺序会出问题
3.信用问题：控制反转