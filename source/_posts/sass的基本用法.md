---
title: Sass的基本用法
date: 2018-07-18 15:47:46
tags:
    - CSS
    - Sass
    - Scss
cover: /assets/four.jpg
---
{% blockquote %}
本文参考来源于 慕课网
{% endblockquote %}

## CSS预处理器简介

<font color="#dd0000">CSS预处理器</font>是用一种专门的编程语言，进行 Web 页面样式设计，然后再编译成正常的 CSS 文件，以供项目使用。CSS 预处理器为 CSS 增加一些编程的特性，使得无需考虑浏览器的兼容性问题～
当前主流的CSS预处理器有这几种：
<table><tr><td bgcolor=#D1EEEE> · Sass（SCSS）
· LESS
· Stylus
· Turbine
· Swithch CSS
· CSS Cacheer
· DT CSS
</td></tr></table>

## Sass简介

Sass 是采用Ruby语言编写的一款CSS预处理语言，它诞生于2007年，是最大的成熟的CSS预处理语言。最初它是为了配合HAML（一种缩进式HTML预编译器）而设计的，因此有着和HTML一样的缩进式风格～

### Sass和SCSS有什么区别？

1.文件扩展名不同，Sass 是以“.sass”后缀为扩展名，而 SCSS 是以“.scss”后缀为扩展名～
2.语法书写方式不同，Sass 是以严格的缩进式语法规则来书写，不带大括号({})和分号(;)，而 SCSS 的语法书写和我们的 CSS 语法书写方式比较类似～
<table><tr><td bgcolor=#D1EEEE> 🌰：Sass 语法
$font-stack: Helvetica, sans-serif  //定义变量
$primary-color: #333 //定义变量
body
  font: 100% $font-stack
  color: $primary-color
🌰：SCSS 语法
$font-stack: Helvetica, sans-serif;
$primary-color: #333;
body {
  font: 100% $font-stack;
  color: $primary-color;
}
//编译后的CSS
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
</td></tr></table>

## Sass基础知识

### 变量声明与调用

声明：
Sass 的变量包括三个部分：
<table><tr><td bgcolor=#D1EEEE> 1. 声明变量的符号“$”
2. 变量名称
3. 赋予变量的值
🌰：$width:200px;
</td></tr></table>sass 的默认变量只需要在它的值后面加上 <font color="#dd0000">!default </font>即可（默认变量可以根据需求来覆盖的，只需要在默认变量之前重新声明下变量即可）～
<table><tr><td bgcolor=#D1EEEE> 🌰：$width:200px !default;
</td></tr></table>调用：
<table><tr><td bgcolor=#D1EEEE> 🌰：$btn-primary-color: #fff !default;
.btn-primary {
   color: $btn-primary-color;
}
</td></tr></table>

### 局部变量和全局变量

<table><tr><td bgcolor=#D1EEEE> 🌰：//SCSS
$color: orange !default;//定义全局变量(在选择器、函数、混合宏...的外面定义的变量为<font color="#dd0000">全局变量</font>)
.block {
  color: $color;//调用全局变量
}
em {
  $color: red;//定义局部变量
  a {
    color: $color;//调用局部变量
  }
}
span {
  color: $color;//调用全局变量
}
//编译后的CSS
.block {
  color: orange;
}
em a {
  color: red;
}
span {
  color: orange;
}
</td></tr></table>通过上面的例子，可以看出，在元素内部定义的变量不会影响其他元素，因此可以理解为，全局变量就是定义在元素外面的变量～
而定义在元素内部的变量，如 $color:red; 它就是一个局部变量,并且当在局部范围（选择器内、函数内、混合宏内...）声明一个已经存在于全局范围内的变量时，局部变量就成为了全局变量的影子；基本上，<font color="#dd0000">局部变量只会在局部范围内覆盖全局变量</font>～
关于什么时候声明变量？
这里建议：
<table><tr><td bgcolor=#D1EEEE> 1. 该值至少重复出现了两次；
2. 该值至少可能会被更新一次；
3. 该值所有的表现都与变量有关（非巧合）。
</td></tr></table>

### 混合宏

在 Sass 中，使用<font color="#dd0000">“@mixin”</font>来声明一个混合宏，并且使用<font color="#dd0000">“@include”</font>来调用声明好的混合宏~
混合宏<font color="#dd0000">可不带参数，带一个参数，带一个参数但参数不设值，带参数且参数带有默认值，或者传多个参数(可用“…”代替，例：$shadows...))</font>～
混合宏在给我们带来极大方便之时，也有不足之处，它会生成冗余的代码块(在调用相同的混合宏时，并不能智能的将相同的样式代码块合并在一起)～
<table><tr><td bgcolor=#D1EEEE> 🌰：@mixin border-radius($radius:5px){
    -webkit-border-radius: $radius;
    border-radius: $radius;
}
button {                              
    @include border-radius;
}     
//编译后的css
button {
  -webkit-border-radius: 3px;
  border-radius: 3px;
}    
</td></tr></table>

### 继承/扩展

在 Sass 中是通过关键词 <font color="#dd0000">“@extend”</font>来继承已存在的类样式块，从而实现代码的继承。
<table><tr><td bgcolor=#D1EEEE> 🌰：//SCSS
.btn {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}
.btn-primary {
  background-color: #f36;
  color: #fff;
  @extend .btn;
}
.btn-second {
  background-color: orange;
  color: #fff;
  @extend .btn;
}
//编译后的CSS
.btn, .btn-primary, .btn-second {    //可看出编译出来的CSS会将选择器合并在一起，形成组合选择器
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
}
.btn-primary {
  background-color: #f36;
  color: #fff;
}
.btn-second {
  background-clor: orange;
  color: #fff;
}
</td></tr></table>

### 占位符/%placeholder

特点：<font color="#dd0000">%placeholder</font> 声明的代码，如果不被 <font color="#dd0000">@extend </font>调用的话，不会产生任何代码, 且调用后编译出来的代码会将相同的代码合并在一起~
<table><tr><td bgcolor=#D1EEEE> 🌰：//SCSS
%mt5 {
  margin-top: 5px;
}
%pt5{
  padding-top: 5px;
}
.btn {
  @extend %mt5;
  @extend %pt5;
}
.block {
  @extend %mt5;

  span {
    @extend %pt5;
  }
}
//编译后的CSS
.btn, .block {
  margin-top: 5px;
}
.btn, .block span {
  padding-top: 5px;
}
</td></tr></table>

### 注释

Sass的注释方式有两种：
<table><tr><td bgcolor=#D1EEEE> 1、使用 ”/* ”开头，结属使用 ”*/ ”～
2、使用“//”～
两者的区别为：第一种会在编译后的css文件中出现，而第二种并不会编译出来～
</td></tr></table>

### 数据类型

Sass中包含这几种数据类型：<font color="#dd0000">数字 </font>，<font color="#dd0000">字符串</font>，<font color="#dd0000">颜色</font>，<font color="#dd0000">布尔型</font>，<font color="#dd0000">空值</font>，<font color="#dd0000">值列表</font>～

#### 字符串

字符串数据类型分为两种：有引号字符串，无引号字符串～
在编译 CSS 文件时不会改变其类型。只有一种情况例外，使用 <font color="#dd0000">#{ }插值语句 </font> 时，有引号字符串将被编译为无引号字符串～

#### 值列表

值列表 (lists) 是指 Sass 如何处理 CSS 中类似：<font color="#dd0000">margin: 10px 15px 0 0</font>这样通过空格或者逗号分隔的一系列的值～(当然独立的值也被视为值列表，它是只包含一个值的值列表，并且值列表中还可以再包含值列表)
值列表具有一下几个功能：(具体用法我们后面再讲～)
    1. nth函数：可以直接访问值列表中的某一项～
    2. join函数：可以将多个值列表连结在一起～
    3. append函数：可以在值列表中添加值～
    4. @each规则：则能够给值列表中的每个项目添加样式～

## Sass基本运算

Sass的运算方法包括：<font color="#dd0000">加</font>，<font color="#dd0000">减</font>，<font color="#dd0000">乘</font>，<font color="#dd0000">除</font>，<font color="#dd0000">变量计算</font>，<font color="#dd0000">数字运算</font>，<font color="#dd0000">颜色运算</font>，以及<font color="#dd0000">字符运算</font>
<font color="#dd0000">加减法</font>：在变量或属性均可使用～
<table><tr><td bgcolor=#D1EEEE> 🌰：//SCSS
.box {
  width: 20px + 8in;
}
//编译后的CSS:
.box {
  width: 788px;
}
//但对于携带不同类型的单位时，在 Sass 中计算会报错(减法亦是如此～)
.box {
  width: 20px + 1em;
}
编译的时候，编译器会报错：“Incompatible units: 'em' and ‘px'.”
</td></tr></table><font color="#dd0000">乘法</font>：进行乘法运算时，两个值单位相同时，只需要为一个数值提供单位即可(若两个值都带有单位，编译时会报错)，当然携带不同类型的单位时，同样会报错～
<font color="#dd0000">除法</font>：与乘法类似，但两个值带有相同的单位值进行除法运算时，会得到一个不带单位的数值～
在纯css的情况下进行除法运算时需在外面添加一个小括号()，否则不生效，但也有不用加()的情况～
不用加()的情况如下：
• 如果数值或它的任意部分是存储在一个变量中或是函数的返回值～
• 如果数值被圆括号包围～
• 如果数值是另一个数学表达式的一部分～
<font color="#dd0000">变量计算</font>，<font color="#dd0000">数字运算</font>：顾名思义，就是变量，数字间的基本运算，就不过多解释了～
<font color="#dd0000">颜色运算</font>：所有算数运算都支持颜色值，并且是分段运算的～
<table><tr><td bgcolor=#D1EEEE> 🌰：//SCSS
p {
  color: #010203 + #040506;
}
计算公式为 01 + 04 = 05、02 + 05 = 07 和 03 + 06 = 09， 并且被合成为颜色值(乘除也可)～
//编译后的CSS
p {
  color: #050709;
}
</td></tr></table><font color="#dd0000">字符运算</font>:在Sass可以直接通过‘+’把字符连接在一起，也可在变量中使用“+”来对字符串进行连接～
<table><tr><td bgcolor=#D1EEEE><font color="#dd0000">注</font>：如果有引号的字符串被添加了一个没有引号的字符串（即带引号的字符串在 + 符号左侧），结果会是一个有引号的字符串。
同样的，如果一个没有引号的字符串被添加了一个有引号的字符串（没有引号的字符串在 + 符号左侧），结果将是一个没有引号的字符串～</td></tr></table>

## Sass控制命令
### @if
<table><tr><td bgcolor=#D1EEEE>@if 指令是一个 SassScript，它可以根据条件来处理样式块，如果条件为 true 返回一个样式块，反之 false 返回另一个样式块～
在 Sass中除了 @if 之，还可以配合 @else if 和 @else 一起使用～</td></tr></table>

### @for循环
<table><tr><td bgcolor=#D1EEEE>在 Sass 中，可以使用 @for 循环来完成～
@for 循环中有两种方式：
·@for $i from <start> through <end>
·@for $i from <start> to <end>
其中$i 表示变量，start 表示起始值，end 表示结束值
这两个的区别是关键字 through 表示包括 end 这个数，而 to 则不包括 end 这个数～</td></tr></table>

### @which循环
<table><tr><td bgcolor=#D1EEEE>@while 指令也需要 SassScript 表达式（像其他指令一样），并且会生成不同的样式块，直到表达式值为 false 时停止循环。这个和 @for 指令很相似，只要 @while 后面的条件为 true 就会执行～</td></tr></table>

### @each循环
<table><tr><td bgcolor=#D1EEEE>@each 循环就是去遍历一个列表，然后从列表中取出对应的值～
@each 循环指令的形式：@each $var in <list> </td></tr></table>

## Sass函数
<table><tr><td bgcolor=#D1EEEE>在 Sass 中除了可以定义变量，具有 @extend、%placeholder 和 mixins 等特性之外，还自备了一系列的函数功能。其主要包括：
·字符串函数
·数字函数
·列表函数
·颜色函数
·Introspection 函数
·三元函数等
当然除了自备的函数功能之外，我们还可以根据自己的需求定义函数功能，常常称之为自定义函数～ </td></tr></table>

### 字符串函数

主要包含的函数：unquote()；quote()；To-upper-case()；To-lower-case()
<font color="#dd0000">unquote($string)</font>：删除字符串中的引号～
注：如果这个字符串没有带有引号，将返回原始的字符串；若带有多个引号，只能删除字符串最前和最后的引号（双引号或单引号），而无法删除字符串中间的引号～
<font color="#dd0000">quote($string)</font>：给字符串添加引号～
注：如果字符串，自身带有引号会统一换成双引号 ""；函数只能给字符串增加双引号，而且字符串中间有单引号或者空格或者碰到特殊符号，比如： !、?、> 等(除中折号 - 和 下划线_)时 ，需要用单引号或双引号括起，否则编译的时候将会报错～
<font color="#dd0000">To-upper-case()</font>：函数将字符串小写字母转换成大写字母～
<font color="#dd0000">To-lower-case()</font>：函数将字符串大写字母转换成小写字母～

### 数字函数

主要包含的函数：percentage($value)；round($value)；ceil($value)；floor($value)；abs($value)；min($numbers…)；max($numbers…)；random()；
<font color="#dd0000">percentage()</font>：主要是将一个不带单位的数字转换成百分比形式～
注：如果转换的值是一个带有单位的值，那么在编译的时候会报错误信息～
<font color="#dd0000">round()</font>：可以将一个数四舍五入为一个最接近的整数～
注：在round() 函数中可以携带单位的任何数值～
<font color="#dd0000">ceil()</font>：将一个数转换成最接近于自己的整数，会将一个大于自身的任何小数转换成大于本身 1 的整数(只做入，不做舍)~
<font color="#dd0000">floor()</font>:刚好与 ceil() 函数功能相反，其主要将一个数去除其小数部分，并且不做任何的进位（只做舍，不做入～
<font color="#dd0000">abs()</font>：会返回一个数的绝对值～
<font color="#dd0000">min()</font> ：在多个数之中找到最小的一个，这个函数可以设置任意多个参数～
<font color="#dd0000">max()</font>：和 min() 函数一样，不同的是，用来获取一系列数中的最大那个值～
<font color="#dd0000">random()</font> ：用来获取一个随机数～

### 列表函数

主要包含的函数：length($list)；nth($list, $n)；join($list1, $list2, [$separator])；append($list1, $val, [$separator])；zip($lists…)；index($list, $value)
<font color="#dd0000">length()</font>：主要用来返回一个列表中有几个值～
<font color="#dd0000">nth()</font>：语法：nth($list,$n)；用来指定列表中某个位置的值；其中的n为1是指列表中的第一个标签值，2是指列给中的第二个标签值，依此类推～
<font color="#dd0000">join() </font>：将两个列表连接合并成一个列表(若连接两个以上的列表，则会报错，但可以将多个join一起使用～)
注：在 join() 函数中还有一个很特别的第三个参数 $separator，这个参数主要是用来给列表函数连接列表值时使用的分隔符号，默认值为 auto，但除了默认值 auto 之外，还有 comma(,) 和 space(空格)) 两个值～
在 join() 函数中除非明确指定了 $separator值，否则将会有多种情形发生，所以建议大家使用时明确指定它的值～
<font color="#dd0000">append() </font>用来将某个值插入到列表中，并且处于最末位～
注：同样具有$separator参数～
<font color="#dd0000">zip()</font>将多个列表值转成一个多维的列表～
注：在使用zip()函数时，每个单一的列表个数值必须是相同的，否则会报错～
<font color="#dd0000">index()</font>类似于索引一样，找到某个值在列表中所处的位置～但在Sass中，第一个值就是1，第二个值就是2，依此类推～
注：若指定的值不在列表中，则返回false～

### Introspection函数

主要包含的函数：type-of($value)；unit($number)；unitless($number)；comparable($number-1, $number-2)
<font color="#dd0000">type-of()</font>：主要用来判断一个值是属于什么类型～
返回值有：number 为数值型；string 为字符串型；bool 为布尔型；color 为颜色型～
<font color="#dd0000">unit()</font>：主要是用来获取一个值所使用的单位，碰到复杂的计算时，其能根据运算得到一个“多单位组合”的值(但只充许乘、除运算)～
<font color="#dd0000">unitless()</font>：用来判断一个值是否带有单位，如果不带单位返回的值为 true，带单位返回的值为 false～
<font color="#dd0000">comparable()</font>：主要是用来判断两个数是否可以进行“加，减”以及“合并”，如果可以返回的值为true，如果不可以返回的值是false～
<font color="#dd0000">Miscellaneous</font>：称为三元条件函数，它有两个值，当条件成立时返回第一种值，当条件不成立时返回另一种值～
语法：if($condition,$if-true,$if-false)

### Maps函数

map常常被称为数据地图，也有人称其为数组，它总是以 key:value 成对的出现，但其更像是一个JSON数据～
<table><tr><td bgcolor=#D1EEEE> 
🌰：map的一般结构：$map: (
                $key1: value1,
                $key2: value2,
                $key3: value3
                    )
</td></tr></table>
map 自身带了七个函数：map-get($map,$key)；map-merge($map1,$map2)；map-remove($map,$key)；map-keys($map)；map-values($map)；map-has-key($map,$key)；keywords($args)；
<font color="#dd0000">map-get($map,$key)</font>：根据$key参数，返回$key在$map中对应的value值，如果$key不存在 $map中，将返回null值～
<font color="#dd0000">map-has-key($map,$key)</font>：将返回一个布尔值，当$map中有这个$key，则函数返回true，否则返回false～
<font color="#dd0000">map-keys($map)</font>：将会返回$map中的所有key～
<font color="#dd0000">map-values($map)</font>获取$map的所有value值，如果有相同的value也将会全部获取出来～
<font color="#dd0000">map-merge($map1,$map2)</font>：将$map1和$map2合并，然后得到一个新的$map～
注：如果$map1和$map2中有相同的$key名，那么将$map2中的$key会取代$map1中的～
<font color="#dd0000">map-remove($map,$key)</font>：用来删除当前$map中的某一个$key，从而得到一个新的map，其返回的值还是一个 map，它不能直接从一个map中删除另一个 map，仅能通过删除 map中的某个key得到新map～
注：如果删除的key并不存在于$map 中，那么返回的新map和以前的map一样～
<font color="#dd0000">keywords($args)</font>：一个动态创建map的函数，可以通过混合宏或函数的参数变创建map,参数也是成对出现，其中$args变成key(会自动去掉$符号)，而$args对应的值就是value~
<table><tr><td bgcolor=#D1EEEE> 
🌰：@mixin map($args...){
    @debug keywords($args);
}
@include map(
  $dribble: #ea4c89,
  $facebook: #3b5998,
  $github: #171515,
  $google: #db4437,
  $twitter: #55acee
);
在命令终端可以看到一个输入的 @debug 信息：
DEBUG: (dribble: #ea4c89, facebook: #3b5998, github: #171515, google: #db4437, twitter: #55acee)
</td></tr></table>

### RGB颜色函数

RGB颜色只是颜色中的一种表达式，其中 R 是 red 表示红色，G 是 green 表示绿色而 B 是 blue 表示蓝色～
RGB 颜色包含六种函数：rgb($red,$green,$blue)；rgba($red,$green,$blue,$alpha)；red($color)；green($color)；blue($color)；mix($color-1,$color-2,[$weight])
<font color="#dd0000">rgb($red,$green,$blue)</font>：根据红、绿、蓝三个值创建一个颜色～
<font color="#dd0000">rgba($red,$green,$blue,$alpha)</font>：主要用来将一个颜色根据透明度转换成 rgba 颜色～
<font color="#dd0000">red($color)；green($color)；blue($color)</font>：主要用来获取一个颜色当中的红色值，绿色值，或蓝色值～
<font color="#dd0000">mix($color-1,$color-2,[$weight])</font>：将两种颜色根据一定的比例混合在一起，生成另一种颜色～
注：$color-1 和 $color-2 指的是你需要合并的颜色，颜色可以是任何表达式，也可以是颜色变量～
$weight为合并的比例（选择权重），默认值为 50%，其取值范围是 0~1 之间。它由每个RGB的百分比来衡量，当然透明度也会有一定的权重，如果指定的比例是25%，这意味着第一个颜色所占比例为25%，第二个颜色所占比例为75%～

### HSL函数

HSL函数主要包括：hsl($hue,$saturation,$lightness)；hsla($hue,$saturation,$lightness,$alpha)；hue($color)；saturation($color)；lightness($color)；adjust-hue($color,$degrees)；lighten($color,$amount)；darken($color,$amount)；saturate($color,$amount)；desaturate($color,$amount)；grayscale($color)；complement($color)；invert($color)
<font color="#dd0000">hsl($hue,$saturation,$lightness)</font>：通过色相（hue）、饱和度(saturation)和亮度（lightness）的值创建一个颜色～
<font color="#dd0000">hsla($hue,$saturation,$lightness,$alpha)</font>：通过色相（hue）、饱和度(saturation)、亮度（lightness）和透明（alpha）的值创建一个颜色～
<font color="#dd0000">hue($color)</font>：从一个颜色中获取色相（hue）值～
<font color="#dd0000">saturation($color)</font>：从一个颜色中获取饱和度（saturation）值～
<font color="#dd0000">lightness($color)：</font>从一个颜色中获取亮度（lightness）值～
<font color="#dd0000">adjust-hue($color,$degrees)</font>：通过改变一个颜色的色相值，创建一个新的颜色，它需要一个颜色和色相度数值，通常这个度数值是在 -360deg 至 360deg 之间，当然了可以是百分数～
<font color="#dd0000">lighten($color,$amount)，darken($color,$amount)</font>：lighten() 和 darken()两个函数都是围绕颜色的亮度值做调整的，其中 lighten() 函数会让颜色变得更亮，与之相反的 darken() 函数会让颜色变得更暗，这个亮度值可以是 0~1 之间，不过常用的一般都在 3%~20% 之间～
注：当颜色的亮度值接近或大于 100%，颜色会变成白色；反之颜色的亮度值接近或小于 0 时，颜色会变成黑色～
<font color="#dd0000">saturate($color,$amount)，desaturate($color,$amount)</font>这两个函数是通过改变颜色的饱和度来得到一个新的颜色，它们和上面介绍的修改亮度得到新颜色的方法非常相似～
<font color="#dd0000">grayscale($color)</font>：会将颜色的饱和度值直接调至 0%，所以此函数与 desaturate($color,100%) 所起的功能是一样的，一般这个函数能将彩色颜色转换成不同程度的灰色～
注：grayscale() 函数处理过的颜色，其最大的特征就是颜色的饱和度为 0～

### Opacity函数

Opacity函数主要包括的函数：alpha($color) /opacity($color)；rgba($color, $alpha)；opacify($color, $amount) / fade-in($color, $amount)；transparentize($color, $amount) / fade-out($color, $amount)
<font color="#dd0000">alpha($color) /opacity($color)</font>：用来获取一个颜色的透明度值，如果颜色没有特别指定透明度，那么这两个函数得到的值都会是1～
<font color="#dd0000">rgba($color, $alpha)</font>：可以创建一个颜色，同时还可以对颜色修改其透明度，其可以接受两个参数，第一个参数为颜色，第二个参数是你需要设置的颜色透明值～
<font color="#dd0000">opacify($color, $amount) / fade-in($color, $amount)</font>用来对已有颜色的透明度做一个加法运算，会让颜色更加不透明～
注：其接受两个参数，第一个参数是原始颜色，第二个参数是你需要增加的透明度值，其取值范围主要是在 0~1 之间，当透明度值增加到大于 1 时，会以 1 计算，表示颜色不具有任何透明度～
<font color="#dd0000">transparentize($color, $amount) / fade-out($color, $amount)</font>：使颜色更加透明～
注：这两个函数会让透明值做减法运算，当计算出来的结果小于 0 时会以 0 计算，表示全透明～

(以上是粗略总结了一下Sass的基本用法，后续会进行文本修改与增添内容～)

