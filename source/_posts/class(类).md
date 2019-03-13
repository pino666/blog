---
title: ES6 - class的基本用法与继承
tags: 
    - Javascript
    - ES6
    - class
cover: /assets/two.jpg
---
{% blockquote %}
本文参考于ECMAScript 6 入门--阮一峰 http://es6.ruanyifeng.com/#docs/class
{% endblockquote %}

## class的基本用法

在JS语言中，生成实力对象的传统方法是通过构造函数。
而ES6引入了class(类)这个概念，作为对象的模板。通过class关键字，可以去定义类。
实际上，ES6的class可以看作只是一个语法糖，它的绝大部分功能ES5其实都可以做到。

<table><tr><td bgcolor=#D1EEEE>   例： //定义类
    class Point {
        constructor(x, y) {
            this.x = x;
            this.y = y;
        }
   
        toString() {
            return '(' + this.x + ',' + this.y + ')';
        }
    }
</td></tr></table>

<!-- more -->

(上面代码定义了一个”类“，里面包含一个构造方法(constructor),代表实例对象的this关键字，因此可以说，ES5的构造函数Point对应的是ES6的Point类的构造方法。此例子中Point类除了构造方法，还定义了一个toString方法。)
(注：定义“类”的方法的时候，前面不需要加<font color="#dd0000">function</font>关键字，以及方法之间也<font color="#dd0000">不需要逗号隔开</font>，直接把函数定义放进去就ok了～)

### ES6的类，完全可以看作是构造函数的另一种方法。
构造函数的prototype属性，在ES6的“类”上面继续存在。事实上，<font color="#dd0000">类的所有方法都定义在了类的prototype属性上面</font>。因此，在类的实例上面调用方法，其实就是在调用原型上面的方法。

<table><tr><td bgcolor=#D1EEEE>   例： class Point {
        constructor() {}
        toString() {}
        toValue() {}
    }
    //等同于
    Point.prototype = {
        constructor() {},
        toString() {},
        toValue() {},
    };
</td></tr></table>

另外，类的内部定义的所有方法都是不可枚举的。这一点与ES5行为不一样～
### 严格模式
类和模块的内部，默认的就是严格模式，所以就不需要使用<font color="#dd0000">use strict</font>去指定运行模式～
### constructor方法
constructor方法是类的默认方法，通过new命令生成对象实例时自动调用该方法。(一个类必须要有constrictor方法，如果没有显示定义，那么会被默认添加一个空的constroctor方法～)
constructor默认返回实例对象(this)，但是可以指定使他返回另外一个对象～
与ES5一样，1.实例的属性除非显式定义在其本身(即定义在this对象上)，否则都是定义在原型上(即定义在class上)～
          2.类的所有实例都共享一个原型对象～   
### 类不存在变量提升
与ES5不同的是，类不存在变量提升～
### 私有方法与私有属性
ES6不提供私有方法，只能通过变通方法模拟实现～
可分为三种类型：
    1.在命名上加以区分--（例：<font color="#dd0000">_bar</font>，在名字前加上下划线，表示这是一个只限于内部使用的私有方法；这种命名不保险，在类外部，还是可以调用到这个方法～）
    2.将私有方法移出模块(因为模块内部的所有方法都是对外可见的)～
    3.利用Symbol值的唯一性，将私有方法的名字命名为一个Symbol值～
同样，ES6不支持私有属性，为class加私有属性有一个提案，即在属性名字之前加一个#号，例：#x～
### this指向问题
类方法内若含有<font color="#dd0000">this</font>,那么它默认指向类的实例；但是当你单独使用该方法时，会有可能报错～
<table><tr><td bgcolor=#D1EEEE>   例：class Logger {
            printName(name = 'there') {
                this.print('Hellp $(name)');
            }
            print(text) {
                console.log(text);
            }
        }
        const logger = new Logger();
        const { printName } = logger;
        printName();  //报错，'print' undefined
        //因为方法单独使用，this会指向方法运行时所在的环境，因此回找不到print方法而报错
</td></tr></table><table><tr><td bgcolor=#D1EEEE>解决方法可为：
    1.在构造函数中绑定this～
     例：class Logger {
            constructor() {
            this.printName = this.printName.bind(this);
            }
        }
    2.使用箭头函数～
    3.使用Proxy，在获取方法的时候，自动获取this～</td></tr></table>

### name属性
本质上ES6的类只是ES5的构造函数的一层包装，所以函数的许多特想搜会被Class继承，包括name属性～
<table><tr><td bgcolor=#D1EEEE> 例： class Point {}
    Point.name //"Point"
    //name属性总是会返回紧跟在class关键字后面的类名～
</td></tr></table>

### class的静态方法
ES6中的<font color="#dd0000">类相当于实例的原型</font>，所有在类中定义的方法都会被实例继承。
”静态方法“为：在一个方法前加上<font color="#dd0000">static</font>关键字，就表示这个方法不会被实例继承，而是直接通过类来调用～
<table><tr><td bgcolor=#D1EEEE> 例： class Foo {
        static classMethod() {
        return 'hello';
        }
    }
    Foo.classMethod() // 'hello'
    var foo = new Foo();
    foo.classMethod()     // TypeError: foo.classMethod is not a function
</td></tr></table>
(注：<font color="#dd0000">若静态方法中包含this关键字，这个this指的是类，不是实例～</font>)
另外，静态方法可以与非静态方法重名，父类的静态方法可以被子类继承～
静态方法也可从<font color="#dd0000">super</font>对象上调用～

<table><tr><td bgcolor=#D1EEEE> 例： class Foo {
        static classMethod() {
            return 'hello';
        }
    }
    class Bar extends Foo {
        static classMethod() {
            return super.classMethod() + ', too';
        }
    }
    Bar.classMethod() // "hello, too"
</td></tr></table>

### new.target属性
ES6为new引入了一个<font color="#dd0000">new.target</font>属性，一般用于构造函数之中，返回new命令作用于的构造函数～
若函数不是通过new命令调用的，那么new.target会返回undefined，由此可以用此来确定构造函数是怎么调用的～
<table><tr><td bgcolor=#D1EEEE> 例： function Person(name) {
        if (new.target !== undefined) {
            this.name = name;
        } else {
            throw new Error('必须使用 new 命令生成实例');
        }
    }
</td></tr></table>

<table><tr><td bgcolor=#D1EEEE> // 另一种写法
    function Person(name) {
        if (new.target === Person) {
            this.name = name;
        } else {
            throw new Error('必须使用 new 命令生成实例');
        }
    }

    var person = new Person('张三'); // 正确
    var notAPerson = Person.call(person, '张三');  // 报错
class内部调用new.target，返回当前class～
(注：子类继承父类时，new.target会返回子类～在函数外部，使用new.target会报错～)
</td></tr></table>

## Class的继承
class可以通过<font color="#dd0000">extends</font>关键字实现继承(继承父类的属性和方法)～
<table><tr><td bgcolor=#D1EEEE> 例： class Point {}
     class ColorPoint extends Point {}
</td></tr></table>
子类必须在constructor方法中调用super方法，否则新建实例时回报错。因为子类自己的this对象必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其加工，加上自己的实例属性和方法～
若不调用super方法，子类就得不到this对象～
ES6的继承机制与ES5不同，ES5继承的实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面(Parent.apply(this)),而ES6的继承机制是先创造父类的实质对象this(所以必须先调用super方法)，然后再用子类的构造函数修改this～
(注：1.不管有没有显式定义，任何子类都会有constructor方法，若没有定义，会被默认添加；2.在子类的构造函数中，只有调用super之后，才可以使用this，否则会报错；3.父类的静态方法也会被子类继承～)

### Object.getPrototypeOf()方法
Object.getPrototypeOf()方法可以用来从子类上获取父类～
<table><tr><td bgcolor=#D1EEEE> 例：   Object.getPrototypeOf(ColorPoint) === Point    //true
</td></tr></table>
因此，可用此方法判断某个类是否继承了另一个类～

### super关键字
<font color="#dd0000">super</font>关键字有两种使用方法：函数或对象～
1.函数：super作为函数调用时，代表父类的构造函数～(ES6要求，子类的构造函数必须执行一次super函数)
(注：1.super虽代表了父类的构造函数，但是返回的是子类的实例，即super内部的this指的子类；2.作为函数时，super()只能在子类的构造函数之中，用在其他地方回报错～)
2.对象：在普通方法中，指向父类的原型对象，在静态方法中，指向父类～
(注：1.由于super指向父类的原型对象，所以定义在父类实例上的方法或属性是无法通过super调用的；2.ES6规定，在子类普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例；3.用在静态方法中，super将指向父类，而不是父类的原型对象；4.在子类的静态方法中调用父类的方法时，方法内部的this指向当前的子类，而不是子类的实例～)

### class的prototype属性与__proto__属性
1.子类的__proto__属性表示构造函数的继承，总是指向父类～
2.子类prototype属性的__proto__属性表示方法的继承，总是指向父类的prototype属性～
(注：可以理解为作为一个对象，子类的原型(__proto__属性)是父类；作为一个构造函数，子类的原型对象(prototype属性)是父类的原型对象(prototype属性)的实例～)
#### 实例的__proto__属性
子类实例的__proto__属性的__proto__属性，指向父类实例的__proto__属性，即子类的原型的原型，是父类的原型

















<!-- ## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html) -->
