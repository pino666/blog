---
title: JS基础类型-常用方法
date: 2018-07-19 16:22:07
tags:
    - JavaScript
cover: /assets/five.jpg
---
## Number 
<font color="#dd0000">Math.round()</font>:四舍五入～
<font color="#dd0000">Math.ceil():</font>向上取整～
<font color="#dd0000">Math.floor()</font>:向下取整～
<font color="#dd0000">Math.max()</font>:取一组数中的最大值～
<font color="#dd0000">Math.min()</font>:取一组数中的最小值～
<font color="#dd0000">Math.random()</font>:取[0,1)之间的随机数(包含0，不含1)～
<table><tr><td bgcolor=#D1EEEE>   🌰：取某个范围的随机数的函数～
function getRandomArbitrary(min, max) {
  return Math.random() * (max - min) + min;
}
</td></tr></table>//类型转换
<font color="#dd0000">parseInt(string, radix)</font>:解析一个字符串，并返回一个整数(通俗讲就是当作几进制的数来转换)～
注：如果字符串的第一个字符不能被转换为数字，那么 parseFloat() 会返回 NaN
<table><tr><td bgcolor=#D1EEEE> 🌰：[1,2,3].map(parseInt)；//1,NaN,NaN
because:map函数可以传递三个参数(element,index,array);so:map将第一个和第二个参数传递给了parseInt()函数，就像这样1-0，2-1，3-2，因此结果为1,NaN,NaN
</td></tr></table><font color="#dd0000">parseFloat()</font>：返回由一个字符串定义的浮点数～
注：解析会在第一个不能作为浮点值解析的字符上结束
<font color="#dd0000">Number()</font>：把对象的值转换为数字～
注：如果参数是 Date 对象，Number() 返回从 1970 年 1 月 1 日至今的毫秒数；如果对象的值无法转换为数字，那么 Number() 函数返回 NaN
<font color="#dd0000">Number.toFixed()</font>:保留小数点后两位～
<table><tr><td bgcolor=#D1EEEE><font color="#dd0000">记</font>：唯一一个与自身不相等的对象--NaN，并且typeOf(NaN);//number
判断：isNaN()存在问题---isNaN("foo")；//true；(它总会隐式的将参数中的值转换成数字再做判断)
so:可先判断是不是number再使用该方法
function myIsNaN(value) { 
    return typeof value === 'number' && isNaN(value); 
} 
</td></tr></table>

## String
"pinoclay"<font color="#dd0000">.length</font>//8; 返回字符串长度～
"pinoclay"<font color="#dd0000">.charAt(0)</font>//"p"; 返回字符串的第一个字符，若大于字符串长度，则返回空字符""～
"pino-clay"<font color="#dd0000">.indexOf("-")</font>//4; 返回<font color="#dd0000">该字符或该字符第一次出现或该字符串一开始出现的索引位置</font>～
"pino-clay12"<font color="#dd0000">.serach(/[0-9]/)</font>//9; 返回第一次出现数字的索引位置～
"pino-clay12"<font color="#dd0000">.serach(/[A-Z]/)</font>//-1; 返回第一次出现小写字母的索引位置，没有则返回-1～
"pino-clay12<font color="#dd0000">".match(/[0-9]/)</font>//['1',index:'9',input:'pino-clay12']; 返回三个值，第一个为匹配的第一个值，第二个为索引值，第三个为输入字符串～
"pino-clay12"<font color="#dd0000">.match(/[0-9]/g)</font>//[1,2]; 返回所有匹配的值，若没有符合匹配的值，则返回null～
"pino-clay12"<font color="#dd0000">.replace("12","##")</font>//"pino-clay##"; 接受两个参数，使用第二个参数替换掉第一个参数，然后返回～
"pino-clay12"<font color="#dd0000">.replace(/[0-9]/,"#")</font>//"pino-clay#2"; 接受两个参数，使用第二个参数替换匹配的第一个数，然后返回～
"pino-clay12"<font color="#dd0000">.replace(/[0-9]/g,"#")</font>//"pino-clay##"; 接受两个参数，使用第二个参数替换掉所有匹配的数，然后返回(若第二参数为空字符"",则相当于将匹配值去掉,例上一个返回"pino-clay")～
"pino-clay12"<font color="#dd0000">.substring(5,7)</font>//"cl"; 返回索引位置5，6的元素(包含左边，不包含右边)～
"pino-clay12"<font color="#dd0000">.substring(5)</font>//"clay12"; 返回从索引位置5开始一直到末尾的元素(包含5)～
"pino-clay12"<font color="#dd0000">.substring(5,-1)</font>//"pino-"; 返回从索引位置5开始一直到原字符串头部的元素(反向获取，不包含5)～
"pino-clay12"<font color="#dd0000">.slice(5,7)</font>//"cl"; 返回被索引位置5，6所选定的元素(包含左边，不含右边)～
"pino-clay12"<font color="#dd0000">.slice(5)</font>//"clay12"; 返回从从索引位置5开始一直到末尾被选中的的元素(包含5)～
"pino-clay12"<font color="#dd0000">.slice(1,-1)</font>//"ino-clay1"; 
"pino-clay12"<font color="#dd0000">.slice(-3)</font>//"y12"; 返回从末尾被选取的元素(-3即末尾的后三个元素)～
"pino-clay12"<font color="#dd0000">.substr(5,2)</font>//"cl"; 若接受两个参数，则第一个参数代表选取元素的起始索引位置，第二个参数代表要选取的数量(如果第一个参数为负，则表示从末尾开始选取，-1即最后一个元素)～
"pino-clay12"<font color="#dd0000">.substr(5)</font>//"clay12"; 返回从索引位置5开始一直到末尾的元素～ 
"pino-clay12"<font color="#dd0000">.split("-")</font>//["pino","clay12"]; //返回被"-"元素分割后的字符串数组～
"pino-clay12"<font color="#dd0000">.split("-",1)</font>//["pino"]; //第二个参数代表返回数组的长度，本例为1，因此只返回第一个～
"pino-clay12"<font color="#dd0000">.split(/[0-9]/)</font>//["pino-clay","",""]
"pino-clay12"<font color="#dd0000">.toLowercase()</font>//"pino-clay12"; 将所有大写字母变成小写字母返回(若没有大写字母，则返回与原字符串一样)～
"pino-clay12"<font color="#dd0000">.toUpperCase()</font>//"PINO-CLAY12"; 将所有小写字母变成大写字母返回(若没有小写字母，则返回与原字符串一样)～<table><tr><td bgcolor=#D1EEEE>记</font>：因为字符串是类数组，因此它可以借用数组的一些方法(例 join,some,splice等等)，但是<font color="#dd0000">除了Array.prototype.reverse方法(它是对原数组进行修改)</font>
判断：isNaN()存在问题---isNaN("foo")；//true；(它总会隐式的将参数中的值转换成数字再做判断)
so:可先判断是不是number再使用该方法
function myIsNaN(value) { 
    return typeof value === 'number' && isNaN(value); 
} 
</td></tr></table>

## Array

<font color="#dd0000">Array.of()</font>：将一组值转换为数组～
<table><tr><td bgcolor=#D1EEEE> 注：这个参数就是来弥补数组构造函数Array()的不足，Array函数在参数个数为没有参数，一个参数，两个及以上参数时返回结果不一样
🌰： Array() // []；无参数返回空数组
    Array(3) // [, , ,]；一个参数代表指定数组的长度
    Array(3, 11, 8) // [3, 11, 8]；两个以上参数返回由参数组成的新数组
🌰： Array.of(3, 11, 8) // [3,11,8]
    Array.of(3) // [3]
    Array.of(3).length // 1
</td></tr></table><font color="#dd0000">Array.length()</font>：返回数组的长度～
<font color="#dd0000">Array.isArray()</font>：判断一个对象是否为数组～
<font color="#dd0000">Array.map()</font>：返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组，它不会改变原来的数组～
<table><tr><td bgcolor=#D1EEEE> 🌰： [1,2,3].map(x => x * 2); //[2,4,6]
</td></tr></table><font color="#dd0000">Array.pop()</font>：删除并返回数组的最后一个元素~
<table><tr><td bgcolor=#D1EEEE> 🌰： [1,2,3].pop(); //[3]
</td></tr></table><font color="#dd0000">Array.fill()</font>：接受三个参数，第一个参数是用于数组的初始化，为数组的每一个元素都填充第一个参数的值；第二个与第三个参数用来指定填充的起始位置与结束位置(包含左，不含右)～
<table><tr><td bgcolor=#D1EEEE> 🌰： ['a','b','c','d','e'].fill(7, 1, 3)// ['a', 7, '7','d','e']
</td></tr></table>['a','b','c','d','e'].fill(7, 1, 3)// ['a', 7, '7','d','e']
<font color="#dd0000">Array.find()</font>：找出第一个符合条件的数组成员，若没有，返回undefined～
<font color="#dd0000">Array.findIndex()</font>：找出第一个符合条件的数组成员的索引位置，若没有，返回-1～
<font color="#dd0000">Array.from()</font>：将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map～
<table><tr><td bgcolor=#D1EEEE> 注：只要是部署了 Iterator 接口的数据结构，Array.from都能将其转为数组
🌰：Array.from('hello')// ['h', 'e', 'l', 'l', 'o']
</td></tr></table>
<font color="#dd0000">Array.join()</font>：把数组中的所有元素放入一个字符串～
注：join可传入一个separator，表示通过此连接符将元素连接成一个字符串
<font color="#dd0000">Array.keys()</font>：对数组键名的遍历～
<table><tr><td bgcolor=#D1EEEE> 🌰：for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1
</td></tr></table><font color="#dd0000">Array.values()</font>：对数组键值的遍历～
<font color="#dd0000">Array.entries()</font>对数组键值对的遍历～
<font color="#dd0000">Array.push()</font>：可向数组的末尾添加一个或多个元素，并返回新的长度～
注：返回的是length
<font color="#dd0000">Array.some()</font>：用于检测数组中的元素是否满足指定条件，会依次执行数组的每个元素，若有一个元素满足条件，则返回true , 剩余的元素不会再执行检测；若没有满足条件的元素，则返回false～
<font color="#dd0000">Array.every()</font>：用于检测数组所有元素是否都符合指定条件，若数组中检测到有一个元素不满足，则返回 false ，且剩余的元素不会再进行检测；若所有元素都满足条件，则返回 true～
<font color="#dd0000">Array.shift()</font>：用于把数组的第一个元素从其中删除，并返回第一个元素的值～
<font color="#dd0000">Array.unshift()</font>：将一个或多个元素添加到数组的开头，并返回新数组的长度，若调用方法时没有参数，则返回数组的长度～
<font color="#dd0000">Array.slice(start,end)</font>：从已有的数组中返回选定的元素(与字符串的此方法类似)～
<font color="#dd0000">Array.concat()</font>：用于连接两个或多个数组～
<font color="#dd0000">Array.filter()</font>：返回指定数组中符合条件的所有元素～
<font color="#dd0000">Array.reduce()</font>：接收一个函数 callback 作为累加器，数组中的每个值（从左到右）开始合并，最终为一个值。
<table><tr><td bgcolor=#D1EEEE> array.reduce(callback[, initialValue])
reduce() 方法接收 callback 函数，而这个函数包含四个参数：function callback(accumulator, currentValue, currentIndex, array){}
accumulator : 上一次调用回调返回的值，或者是提供的初始值（initialValue）
currentValue : 数组中当前被处理的数组项
currentIndex : 当前数组项在数组中的索引值
array : 调用 reduce() 方法的数组 
而 initialValue 作为第一次调用 callback 函数的第一个参数。
回调函数第一次执行时，accumulator 和 currentValue 可以是一个值，如果 initialValue 在调用 reduce() 时被提供，那么第一个 accumulator 等于 initialValue ，并且 currentValue 等于数组中的第一个值；如果 initialValue 未被提供，那么 accumulator 等于数组中的第一个值，currentValue 等于数组中的第二个值～
🌰：var arr = [0,1,2,3,4];
    arr.reduce(function (accumulator, currentValue, currentIndex, array) {
    return accumulator+ currentValue; }); // 10
</td></tr></table><font color="#dd0000">Array.reduceRight()</font>：功能和 reduce() 功能是一样的，不同的是 reduceRight() 从数组的末尾向前将数组中的数组项做累加～
<font color="#dd0000">Array.splice(index,howmany,item1,.....,itemX)</font>：返回被删除的元素；可接受三个参数，第一个参数代表删除元素的索引位置，删除元素的个数取决于第二个参数的设置，并可以将第三个参数设置的值来替换被删除的元素～
<table><tr><td bgcolor=#D1EEEE> 🌰：var arr = [1,2,3,4,5];
    arr.splice(2,2); //[3,4]
    arr; //[1,2,5]
</td></tr></table><font color="#dd0000">Array.forEach()</font>：用于调用数组的每个元素，并将元素传递给回调函数~
<font color="#dd0000">Array.indexOf()</font>：返回元素在数组中的第一次出现的索引位置或者说在数组中从前向后查找，若没有，返回-1～
<font color="#dd0000">Array.lastIndexOf()</font>：返回元素在数组中最后一次出现的索引位置，或者说在数组中从后向前查找，若没有，返回-1～
<font color="#dd0000">Array.reverse()</font>：将数组元素反转，再返回，并改变的是原有的数组～
<font color="#dd0000">Array.findIndex()</font>：接受一个函数，返回数组中满足此函数的第一个元素的索引位置，否则返回-1～
<table><tr><td bgcolor=#D1EEEE> 🌰：var arr = [1,2,3,4,5];
    arr.findIndex(x => x > 3); //3
</td></tr></table>

## Date
<font color="#dd0000">New Date()</font>：获取当前时间～
<font color="#dd0000">New Date(2018,6,21,11,30)</font>：设置规定日期(月份从0开始)～
<table><tr><td bgcolor=#D1EEEE> 还可以单独设置～
var date = new Date(2018, 7, 18) // 0 date.setFullYear()
date.setMonth(3) // 4
date.setDate(35) // 9 4
date.setHours()
date.setMinutes()
date.setSeconds()
单独获取～
date.getFullYear()
date.getMonth()
date.getDate()
date.getHours()
date.getMinutes()
date.getSeconds()
将时间戳转换为时间～
new Date(12321321312) // Sat May 23 1970 22:35:21 GMT+0800 (中国标准时间)
</td></tr></table>

