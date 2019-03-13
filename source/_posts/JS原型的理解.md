---
title: 简述对JS原型的理解
date: 2018-08-20 16:36:41
tags:
    -JavaScript
    -原型
    -原型链
cover: /assets/eight.jpg
---
{% blockquote %}
本文参考来源于 网络
{% endblockquote %}

### prototype
prototype是函数默认的属性，每个函数都有prototype～
prototype的属性值是一个对象，默认的只有一个叫做constructor的属性，指向这个函数本身，但其属性值是一个对象，即属性的集合，所以不会只存在这一个函数～
<table><tr><td bgcolor=#D1EEEE>🌰：Object的prototype里面，就有好几个其他属性：
<img src="/assets/object.jpg"/>
也可自定义添加属性:
 function Fn() { }
    Fn.prototype.name = 'pino';
    Fn.prototype.getYear = function () {
        return 1994;
    };
</td></tr></table>

### __proto__
上文提到，每个函数都有一个prototype，其实还可以加一个，每个对象都有一个__proto__（隐式原型）～
<font color="#dd0000">注：</font>__proto__属性指向创建该对象的函数的prototype，但是只有一点，请记住，Object.prototype的__proto__指向的是null～

### instanceof
Instanceof接收两个参数，第一个变量是一个对象，暂时称为A；第二个变量一般是一个函数，暂时称为B。
那么，Instanceof的判断规则是：沿着A的__proto__这条线来找，同时沿着B的prototype这条线来找，如果两条线能找到同一个引用，即同一个对象，那么就返回true。如果找到终点还未重合，则返回false。

### 原型图(重点理解)
结合上面三个的说法，可看下面这个整体图进行理解：
<img src="/assets/prototype.jpg"/>

### 继承
javascript中的继承是通过原型链来体现的～
<table><tr><td bgcolor=#D1EEEE>🌰：function Foo () {};
var f1 = new Foo();
f1.a = 10;
Foo.prototype.a = 100;
Foo.prototype.b = 200;
console.log(f1.a);  //10
console.log(f1.b);  //200

以上代码中，f1是Foo函数new出来的对象，f1.a是f1对象的基本属性，那f1.b呢？——从Foo.prototype得来，因为f1.__proto__指向的是Foo.prototype

因此，访问一个对象的属性时，先在基本属性中查找，如果没有，再沿着__proto__这条链向上找，这就是原型链。
</td></tr></table>
但是有一种情况，就是想要知道这个属性是本身的属性还是继承过来的属性，这个应该怎么知道呢？-----答案是：hasOwnProperty(Object.prototype中的属性)
<table><tr><td bgcolor=#D1EEEE>function Person(){}
Person.prototype.name = "allen";
var person = new Person();
console.log(person.hasOwnProperty("name")); //false
</td></tr></table>

