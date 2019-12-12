---
title: leetcode刷题小记(简单版)
date: 2019-11-27 18:10:26
tags:
---
{% blockquote %}
本文参考于题目出自leetcode官方，有些答案参考于网络
{% endblockquote %}

## 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

### 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
<table><tr><td bgcolor=#D1EEEE>   🌰：给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
</td></tr></table>
解法一：
```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    for(let i=0;i< nums.length; i++){
        for(let j = i+1; j< nums.length ;j++){
            if(nums[i] + nums[j] == target){
                return [i,j];
            }
        }
    }
};
```
解法二:
```
var twoSum = function(nums, target) {
    for(let i = 0; i < nums.length; i++){
        let j = nums.indexOf(target - nums[i]);
        if(j != i && j != -1){
            return [i , j];
        }
    }
};
```

## 给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
### 如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。 您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
</td></tr></table>
```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    let result = new ListNode();
    let list = result
    let param = 0;


    while (l1 != null || l2 != null){

        let l1True = l1 != null;
        let l2True = l2 != null;

        let one = l1True ? l1.val : 0;
        let two = l2True ? l2.val : 0;

        let num = (one + two + param) % 10;
        param = Math.floor((one + two + param) / 10);

        list.next = new ListNode(num);
        list = list.next

        if(l1True) l1 = l1.next
        if(l2True) l2 = l2.next

    }
     if(param) {
        list.next = new ListNode(param)
     }

    return result.next
};
```

## 给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
</td></tr></table>
```
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let j = 0 ;
    let len = 0;
    let str = '';
    for(let i of s ){
        if(str.indexOf(i) != -1){
            str = str + i
            j = str.indexOf(i) + 1
            str = str.slice(j)
        }else{
           str = str + i;  
           len = len > str.length ? len : str.length;
        }
        
    }
    return len;
};
```

## 给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
### 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2的31次方,  2的31次方 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: -123
输出: -321
</td></tr></table>
```
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    let result = '';
    if(Math.floor(x/10 ) == 0) return x;
    let y = x < 0 ? -x : x;
    while(y) {
        result += y % 10;
        y = Math.floor(y / 10);
    }
    result = Number(result)
    if(result >= Math.pow(2, 31) - 1 || result <= -Math.pow(2, 31)) return 0;
    result = x < 0 ? -result : result;
    return result;
};
```
## 请你来实现一个 atoi 函数，使其能将字符串转换成整数。
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。
当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。
该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0。
### 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2的31次方,  2的31次方 − 1]。如果数值超过这个范围，请返回  INT_MAX (2的31次方 − 1) 或 INT_MIN (−2的31次方) 。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
</td></tr></table>
```
/**
 * @param {string} str
 * @return {number}
 */
var myAtoi = function(str) {
    let s = str.trim();
    let result = '';
    let positiveNum= '';
    
   if(!Number(s.slice(0,1)) && s.slice(0,1) !== '0') {
       if(s.slice(0,1) == '-' || s.slice(0,1) == '+') {
            if(!Number(s.slice(1,2)) && s.slice(1,2) !== '0') return 0;
            positiveNum = s.slice(0,1)
            s = s.slice(1)
        }else{
          return 0;  
        }   
   }
    
    for(let i of s){
        if((!Number(i) && i !== '0') || i === '.'){
            break;
        };
        result += i;   
    }
    result = Number(positiveNum +  result)
    if(result >= Math.pow(2,31)-1) return Math.pow(2,31)-1;
    if(result <= -Math.pow(2,31)) return -Math.pow(2,31);
    return result;
};
```
## 判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
</td></tr></table>
```
/**
 * @param {number} x
 * @return {boolean}
 * 通过判断整数的两半是否相等，也可直接判断整数反转后是否相等
 */
var isPalindrome = function(x) {
    if(x < 0 ) return false;
    if( x >= 0 && x < 10) return true;
    let n = 1;
    let res = 0;
    // 计算此数字是几位数
    while(Math.floor(x / (Math.pow(10,n)))){
        n++;
    }
    // 算出前半部分数字
    let q = n % 2 ? Math.floor(x/Math.pow(10, (n-1)/2 + 1)) : Math.floor(x/Math.pow(10,n/2));
    // 后半部分数字
    let p = n % 2 ? x % Math.pow(10,(n-1)/2) : x % Math.pow(10,n/2);
    // 将前半部分转化为后半部分
    while(q){
        res = res *10 + q % 10;
        q = Math.floor(q / 10);
    }
    // 判断是否相等
    if(res === p) return true;
    return false;
};
```
## 罗马数字转整数
罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
</td></tr></table>
```
/**
 * @param {string} s
 * @return {number}
 */
var romanToInt = function(s) {
    let sum = 0;
    let rules = {
        'I' : 1,
        'V' : 5,
        'X' : 10,
        'L' : 50,
        'C' : 100,
        'D' : 500,
        'M': 1000,
        'IV':4,
        'IX':9,
        'XL':40,
        'XC':90,
        'CD':400,
        'CM':900,
    }
    for(let i = 0 ; i< s.length;i++){
        if(rules[s.slice(i,i+2)]){
            sum += rules[s.slice(i,i+2)]
            i++;
        }else{
            sum += rules[s.slice(i,i+1)]
        }
       
    }
    return sum;
    
};
```
## 编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 ""。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: ["flower","flow","flight"]
输出: "fl"。
所有输入只包含小写字母 a-z 。
</td></tr></table>
```
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if(strs.length === 1) return strs[0];
    if(strs.length === 0) return "";
    let result = strs[0];
    for(let i = 1 ;i < strs.length;i++){
        let j= 0;
        for(;j<strs[0].length && j< strs[i].length;j++){
            if(strs[0][j] != strs[i][j]){
                break;
            }
        }
        result = result.slice(0, j);
    }
    return  result;
};
```