---
title: this指向
date: 2018-08-17 14:57:26
tags:
    -JavaScript
    -this
cover: /assets/seven.jpg
---
{% blockquote %}
本文参考来源于 网络
{% endblockquote %}

## this的指向问题
    在函数中的this到底取何值，是在函数真正被调用执行时确定的，而不是在函数定义时确定的～
### 全局环境下
在全局模式下，this始终指向全局对象，即window～
<font color="#dd0000">注：</font>无论是否是严格模式，都指向window<table><tr><td bgcolor=#D1EEEE>console.log(this === window); // true
this.a = 1216;
console.log(window.a); // 1216
</td></tr></table>

### 作为普通函数直接调用
在普通函数的内部，this也是指向全局对象(严格模式下指向undefined)～<table><tr><td bgcolor=#D1EEEE>function fn(){
  return this;
}
fn() === window; // true
</td></tr></table>

### 作为对象的属性调用
若函数作为对象的一个属性调用时，this指向该对象～
但若将对象属性赋值给一变量，再调用函数，则this指向window对象～
若多层嵌套的对象，内部this指向离被调用函数最近的对象～<table><tr><td bgcolor=#D1EEEE>var obj = {
	x:10,
	fn:function(){
		console.log(this);
		console.log(this.x);
	}
}
obj.fn();  // {x: 10, fn: ƒ}
          // 10
var fn1 = obj.fn;
fn1();  // Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
        // undefined
</td></tr></table>

### 在构造函数中调用
在构造函数中，this指向即将被new出来的实例对象～<table><tr><td bgcolor=#D1EEEE>function Foo(){
	this.name = 'pino';
	console.log(this);
}
var f1 = new Foo();
console.log(f1.name);   // pino
</td></tr></table>

### call 和 apply
当函数通过call()和apply()方法绑定时，this指向两个方法的第一个参数对象上，若第一个参数不是对象，JS内部会尝试将其转化为对象然后再指向它～<table><tr><td bgcolor=#D1EEEE>var obj = {
	x:10,
	fn:function(){
		console.log(this);
		console.log(this.x);
	}
}
var fn = function(){
	console.log(this);
	console.log(this.x);
}
fn.call(obj);   // {x: 10, fn: ƒ}
                // 10
</td></tr></table>

### bind
通过bind方法绑定后，无论其在什么情况下被调用，函数将被永远绑定在其第一个参数对象上～
bind绑定后返回的是一个函数～<table><tr><td bgcolor=#D1EEEE>function f(){
  return this.a;
}
var g = f.bind({a:"啊啊啊"});
console.log(g()); // 啊啊啊
var o = {a:111, f:f, g:g};
console.log(o.f(), o.g()); // 111, 啊啊啊
</td></tr></table>

### 箭头函数
箭头函数不绑定this， 它会捕获其所在（即定义的位置）上下文的this值， 作为自己的this值～