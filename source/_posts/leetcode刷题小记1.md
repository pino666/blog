---
title: leetcode刷题小记一(简单版)
date: 2019-11-27 18:10:26
tags: -JavaScrip
---
{% blockquote %}
本文参考于题目出自leetcode官方，有些答案参考于网络，仅供学习使用～
{% endblockquote %}

## 1. 两数之和
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。
<table><tr><td bgcolor=#D1EEEE> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素
 🌰：给定 nums = [2, 7, 11, 15], target = 9
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

## 2. 两数相加（中等）
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
<table><tr><td bgcolor=#D1EEEE> 如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。 您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
🌰：输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
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

## 3. 无重复字符的最长子串（中等）
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。
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

## 4. 整数反转 
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
<table><tr><td bgcolor=#D1EEEE> 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2的31次方,  2的31次方 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
🌰：输入: -123
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
## 5.请你来实现一个 atoi 函数，使其能将字符串转换成整数。
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。
当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。
该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0。
<table><tr><td bgcolor=#D1EEEE> 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2的31次方,  2的31次方 − 1]。如果数值超过这个范围，请返回  INT_MAX (2的31次方 − 1) 或 INT_MIN (−2的31次方) 。
🌰：输入: "   -42"
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
## 6. 判断一个整数是否是回文数。
回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
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
## 7. 罗马数字转整数
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
## 8. 最长公共前缀
编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 ""。
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
## 9. 有效的括号
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
有效字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: "()"
输出: true
输入: "()"
输出: true
</td></tr></table>
```
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    if(s === '') return true;
    let arr = [];
    const mapp = { '(' :')', '[':']', '{':'}'};
    for(let i of s){
        if(mapp.hasOwnProperty(i)){
            arr.push(i)
        }else{
            let left = arr.pop()
            if(mapp[left] !== i) {
                return false 
            } 
        }
    }
    if(arr.length) return false;
    return true;
};
```
## 10. 合并两个有序链表
将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的.
<table><tr><td bgcolor=#D1EEEE>   🌰：输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
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
var mergeTwoLists = function(l1, l2) {
  if(l1 === null) return l2;
  if(l2 === null) return l1;

  if(l1.val < l2.val){
      l1.next = mergeTwoLists(l1.next,l2)
      return l1
  }
    l2.next = mergeTwoLists(l1,l2.next)
    return l2
};
```
## 11. 删除排序数组中的重复项
给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
(不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。)
<table><tr><td bgcolor=#D1EEEE>   🌰：给定 nums = [0,0,1,1,1,2,2,3,3,4],
函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
你不需要考虑数组中超出新长度后面的元素.
给定数组 nums = [1,1,2], 
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
你不需要考虑数组中超出新长度后面的元素。
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    if(nums.length === 0) return 0;
    for(let j=0; j<nums.length;){
        if(nums[j] === nums[j+1]){
            nums.splice(j,1);
        }else{
            j++;
        }
    }
    return nums.length
};
||
var removeDuplicates = function(nums) {
    if(nums.length === 0) return 0;
    let length = 1;
    for(let j=1; j<nums.length;j++){
        if(nums[j] !== nums[j-1]){
            nums[length++] = nums[j]
        }
    }
    return length
};
```
## 12.移除元素 
给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。
不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。
元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。
<table><tr><td bgcolor=#D1EEEE>   🌰：给定 nums = [3,2,2,3], val = 3,
函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。
你不需要考虑数组中超出新长度后面的元素
🌰：给定 nums = [0,1,2,2,3,0,4,2], val = 2,
函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。
注意这五个元素可为任意顺序。
你不需要考虑数组中超出新长度后面的元素。
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @param {number} val
 * @return {number}
 */
var removeElement = function(nums, val) {
    let i = 0;
    let len = nums.length;
    while(i < len){
        if(nums[i] == val){
            nums[i] = nums[len-1]
            len--;
        }else{
            i++;
        }
    }
    return len
};
||
var removeElement = function(nums, val) {
    for(let i = 0;i < nums.length;){
        if(nums[i] == val){
            nums.splice(i,1)
        }else{
            i++
        }
       
    }
    return nums.length
};
```
## 13. 实现 strStr() 函数
给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: haystack = "hello", needle = "ll"
输出: 2
🌰：输入: haystack = "aaaaa", needle = "bba"
输出: -1
当 needle 是空字符串时我们应当返回 0 。
</td></tr></table>
```
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function(haystack, needle) {
    if(needle === '') return 0;
    return haystack.indexOf(needle)
};
||
var strStr = function(haystack, needle) {
    if(needle === '') return 0;
    for(let i=0;i<haystack.length;i++){
        if(haystack[i] === needle[0]){
            if(haystack.substr(i,needle.length) === needle) return i
        }
    }
    return -1
};
```
## 14. 搜索插入位置
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
你可以假设数组中无重复元素。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: [1,3,5,6], 5
输出: 2
🌰：输入: [1,3,5,6], 0
输出: 0
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
var searchInsert = function(nums, target) {
     let ret = 0;
    if(nums[0] > target) return 0;
    for(let i=0;i<nums.length;i++){
        if(nums[i] >= target) return i
    }
    return nums.length;
};
||(二分法)
var searchInsert = function(nums, target) {
    let left = 0;
    let right = nums.length - 1;
    if(nums[0] > target) return 0;
    if(nums[nums.length-1] < target) return nums.length;
    while(left <= right){
        let mid = Math.floor(left + (right - left) / 2);
        if(nums[mid] < target){
            left = mid+1;
        }
        else if(nums[mid] > target){
            right = mid - 1;
        }else if(nums[mid] === target){
            return mid;
        }
    }
    return left;
};
```
## 15. 最大子序和
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    if(nums.length === 0 )return 0;
    let sum = 0;
    let ret = nums[0];
    for(let i = 0;i<nums.length;i++){
        if(sum > 0){
            sum = sum + nums[i];
        }else{
            sum=nums[i];
        }
        ret = Math.max(ret,sum);
    }
    return ret;
};
```
## 16. 最后一个单词的长度
给定一个仅包含大小写字母和空格 ' ' 的字符串，返回其最后一个单词的长度。如果不存在最后一个单词，请返回 0 。
说明：一个单词是指由字母组成，但不包含任何空格的字符串。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: "Hello World"
输出: 5
</td></tr></table>
```
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLastWord = function(s) {
    if(s === '')return 0;
    let num = 0;
    let rst = 0;
    for(const str of s){
        if(str === ' '){
            rst = num === 0 ? rst : num 
            num = 0
        }else{
            num +=1;
        }
    } 
    if(!num){
        return rst
    } 
    return num
}
```
## 17. 加一
给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
你可以假设除了整数 0 之外，这个整数不会以零开头。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123
🌰：输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321
</td></tr></table>
```
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var plusOne = function(digits) {
    let rst = 0;
    for(let i = digits.length -1 ; i >= 0; i--){
        digits[i] += 1
        if(digits[i] % 10 !== 0){
            return digits
        }
        digits[i] = digits[i] % 10
        if(i === 0){
            return [1, ...digits]
        }
    }
};
```
## 18. 实现 int sqrt(int x) 函数。
计算并返回 x 的平方根，其中 x 是非负整数。
由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: 4
输出: 2
🌰：输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去
</td></tr></table>
```
/**
 * @param {number} x
 * @return {number}
 */
var mySqrt = function(x) {
    return Math.floor(Math.sqrt(x))
};
||(粗暴法，用时与内存都极不理想)
var mySqrt = function(x) {
    for(let i = 0; i <= x; i++){
        if(i * i <= x && (i+1) * (i+1) > x){
            return i
        }
    }
};
```
## 19.爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
🌰：输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
</td></tr></table>
```
斐波那契数列（可用公式直接求出）
/**
 * @param {number} n
 * @return {number}
 */
var climbStairs = function(n) {
    let arr = [0,1,2]
    for(let i=3;i<=n;i++){
        arr[i] = arr[i-1] + arr[i-2]
    }
    return arr[n]
};
```
## 20. 删除排序链表中的重复元素
给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入: 1->1->2
输出: 1->2
🌰：输入: 1->1->2->3->3
输出: 1->2->3
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
 * @param {ListNode} head
 * @return {ListNode}
 */
var deleteDuplicates = function(head) {
    let result = head
    while(result && result.next){
        if(result.val === result.next.val){
            result.next = result.next.next
        } else{
             result = result.next  
        }    
    }
    return head
};
```
## 21. 判断相同的树
给定两个二叉树，编写一个函数来检验它们是否相同。
如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。
<table><tr><td bgcolor=#D1EEEE>   🌰：输入:       
           1         1
          / \       / \
         2   3     2   3
        [1,2,3],   [1,2,3]

输出: true
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function(p, q) {
    if(!p && !q){
        return true;
    }else if(!p || !q ){
        return false;
    }else if(p.val !== q.val){
        return false;
    }
    return isSameTree(p.left,q.left) && isSameTree(p.right,q.right)
};
```
## 22.  判断对称二叉树
给定一个二叉树，检查它是否是镜像对称的。
<table><tr><td bgcolor=#D1EEEE>   🌰：例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```
             1
            / \
           2   2
          / \ / \
         3  4 4  3
```
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```
            1
           / \
          2   2
           \   \
            3   3
```
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isSymmetric = function(root) {
    if(!root) return true;
    return checkTree(root.left,root.right)
};
const checkTree = function(left,right) {
    if(!left && !right) return true;
    if(!left || !right) return false;
    if(left.val === right.val){
        return checkTree(left.left,right.right) && checkTree(left.right,right.left)
    }
    return false
};
```
## 23.二叉树的最大深度
给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。
<table><tr><td bgcolor=#D1EEEE>   🌰：给定二叉树 [3,9,20,null,null,15,7]，
```
         3
        / \
        9  20
          /  \
        15   7
```
返回它的最大深度 3 。
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxDepth = function(root) {
    if(!root) return 0;
    let num = Math.max( maxDepth(root.left), maxDepth(root.right) ) + 1;
    return num;
};
```
## 24. 二叉树的层次遍历 II
给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）
<table><tr><td bgcolor=#D1EEEE>   🌰：例如：给定二叉树 [3,9,20,null,null,15,7],
```
         3
        / \
        9  20
          /  \
        15   7
```
返回其自底向上的层次遍历为：
```
      [
        [15,7],
        [9,20],
        [3]
      ]
```
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrderBottom = function(root) {
    if(!root) return [];
    let arr = [];
    const checkTree = function(root, index){
        if(!root) return;
        arr[index] = arr[index] || [];
        arr[index].push(root.val)
        checkTree(root.left, index+1);
        checkTree(root.right, index+1);
    }
    checkTree(root, 0);
    return arr.reverse();
    
};
```
## 25. 平衡二叉树
给定一个二叉树，判断它是否是高度平衡的二叉树。
<table><tr><td bgcolor=#D1EEEE> 本题中，一棵高度平衡二叉树定义为：
一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。
🌰：给定二叉树 [3,9,20,null,null,15,7]
```
        3
       / \
      9  20
        /  \
       15   7
```
返回 true 。
给定二叉树 [1,2,2,3,3,null,null,4,4]
```
         1
        / \
       2   2
          / \
         3   3
        / \
       4   4
```
返回 false 。
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
const balanceTree = (node) => {
    if (!node) return 0
    return Math.max(balanceTree(node.left), balanceTree(node.right)) + 1;
}
var isBalanced = function(root) {
    if(!root) return true;
    if(Math.abs(balanceTree(root.left) - balanceTree(root.right)) > 1) return false;
    if(isBalanced(root.left) && isBalanced(root.right)) return true;
    return false;
};
```
## 26. 二叉树的最小深度
给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
<table><tr><td bgcolor=#D1EEEE> 说明: 叶子节点是指没有子节点的节点。
🌰：给定二叉树 [3,9,20,null,null,15,7],
```
        3
       / \
      9  20
        /  \
       15   7
```
返回它的最小深度  2.
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var minDepth = function(root) {
    if(!root) return 0;
    if(!root.left || !root.right) return Math.max(minDepth(root.left),minDepth(root.right))+1;
    return Math.min(minDepth(root.left),minDepth(root.right))+1;
   
};
```
## 27. 求路径总和
给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。
说明: 叶子节点是指没有子节点的节点。
<table><tr><td bgcolor=#D1EEEE> 说明: 叶子节点是指没有子节点的节点。
🌰：给定如下二叉树，以及目标和 sum = 22，
```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} sum
 * @return {boolean}
 */
var hasPathSum = function(root, sum) {
    if(!root) return false;
    if(!root.left && !root.right) return root.val === sum;
    return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
};
```
## 28. 杨辉三角
给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。
在杨辉三角中，每个数是它左上方和右上方的数的和。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 5
```
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```
</td></tr></table>
```
/**
 * @param {number} numRows
 * @return {number[][]}
 */
var generate = function(numRows) {
    let arr = [];
    for(let i=0; i<= rowIndex; i++){
        let res = [];
        for(let j=0;j<=i;j++){
            if(j=== 0 || j===i){
                res.push(1);
                continue;
            }
            res.push(arr[i-1][j-1] + arr[i-1][j]);
        }
        arr.push(res);
    }
    return arr;
};
```
## 29. 杨辉三角 II
给定一个非负索引 k，其中 k ≤ 33，返回杨辉三角的第 k 行。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 3
输出: [1,3,3,1]
</td></tr></table>
```
/**
 * @param {number} rowIndex
 * @return {number[]}
 */
var getRow = function(rowIndex) {
    let arr = [];
    for(let i=0; i<= rowIndex; i++){
        let res = [];
        for(let j=0;j<=i;j++){
            if(j=== 0 || j===i){
                res.push(1);
                continue;
            }
            res.push(arr[i-1][j-1] + arr[i-1][j])
        }
        arr.push(res)
    }
    return arr[rowIndex]
};
```
## 30. 买卖股票的最佳时机
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。
<table><tr><td bgcolor=#D1EEEE>注意你不能在买入股票前卖出股票。
🌰：输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
🌰：输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
</td></tr></table>
```
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    if(!prices.length) return 0;
    let p = q = prices[0];
    let res = 0;
    for(let i = 1;i< prices.length; i++){
        if(prices[i] < p) {
            p = q = prices[i];
            continue;
        }
        if(prices[i] > q){
            q = prices[i];
            res = q - p > res ? q - p : res;
        }
    }
    return res;
};
```
## 31. 买卖股票的最佳时机 II
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
<table><tr><td bgcolor=#D1EEEE>注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
🌰：输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
🌰：输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
</td></tr></table>
```
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    if(!prices.length) return 0;
    let p = q = prices[0];
    let res = 0;
    for(let i = 1;i< prices.length; i++){
        if(prices[i] < p) {
            p = q = prices[i];
            continue;
        }
        if(prices[i] > q){
            if(i === prices.length-1)  return res+prices[i]-p;
            if(prices[i+1] < prices[i]){
                res = res + prices[i] - p;
                q = p = prices[i];
            }            
        }
    }
    return res;
};
```
## 32. 验证回文串
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。
<table><tr><td bgcolor=#D1EEEE>说明：本题中，我们将空字符串定义为有效的回文串。
🌰：输入: "A man, a plan, a canal: Panama"
输出: true
🌰：输入: "race a car"
输出: false。
</td></tr></table>
```
/**
 * @param {string} s
 * @return {boolean}
 */
var isPalindrome = function(s) {
    let str = s.replace(/[^0-9a-zA-Z]/g,'').toLowerCase();
    let i=0;
    let j= str.length-1;
    let middle = Math.ceil(str.length / 2);
    while(i < middle && j > middle-1) {
        if(str[i] !== str[j]) return false;
        i++;
        j--;
    }
    return true;
};
```
## 33. 只出现一次的数字
给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
<table><tr><td bgcolor=#D1EEEE>说明：你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？
🌰：输入: [2,2,1]
输出: 1
🌰：输入: [4,1,2,1,2]
输出: 4
</td></tr></table>
```
解题思路：使用异或（a 异或 0 = a，a 异或 b 异或 a = b，a 异或 a = 0）
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function(nums) {
    let res = 0;
    for(num of nums) {
        res ^= num
    }

    return res;
};
```
## 34. 环形链表
给定一个链表，判断链表中是否有环。
为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。
<table><tr><td bgcolor=#D1EEEE>🌰：输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
🌰：输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
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
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {
    if(!head || !head.next) return false;
    while(head){
        if(head.val === 'have') return true;
        head.val = 'have';
        head = head.next;
    }
    return false;
};
||
var hasCycle = function(head) {
    if(!head || !head.next) return false;
    let slow = head;
    let fast = head.next;
    while(slow !== fast){
        if(!fast || !fast.next) return false;
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
};
```
## 35. 最小栈
设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。
·push(x) -- 将元素 x 推入栈中。
·pop() -- 删除栈顶的元素。
·top() -- 获取栈顶元素。
·getMin() -- 检索栈中的最小元素。
<table><tr><td bgcolor=#D1EEEE>🌰：MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
</td></tr></table>
```
/**
 * initialize your data structure here.
 */
var MinStack = function() {
    this.arr = [];
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
    this.arr[this.arr.length] = x;
    return this.arr;
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    let num = this.arr[this.arr.length-1];
    this.arr.length = this.arr.length-1;
    return num;
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.arr[this.arr.length-1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    let res = this.arr[0];
    for(let num of this.arr){
        res = res < num ? res : num;
    }
    return res;
};

/** 
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```
## 36.  两数之和 II - 输入有序数组
给定一个已按照升序排列 的有序数组，找到两个数使得它们相加之和等于目标数。
函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。
说明:
返回的下标值（index1 和 index2）不是从零开始的。
你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
</td></tr></table>
```
/**
 * @param {number[]} numbers
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(numbers, target) {
    for(let i = 0; i<numbers.length; i++){
        for(let j = i+1; j < numbers.length; j++){
            if(numbers[i] + numbers[j] === target) return [i+1, j+1];
        }
    }
};
||
var twoSum = function(numbers, target) {
    let small = 0; 
    let big = numbers.length;
    while(small < big){
        if(numbers[small] + numbers[big] === target) return [small+1, big+1];
        if(numbers[small] + numbers[big] < target) { 
            small++;
        }else{
            big--;
        }
    }
};
```
## 37. Excel表列名称
给定一个正整数，返回它在 Excel 表中相对应的列名称。
例如，

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 1
输出: "A"
🌰：输入: 28
输出: "AB"
🌰：输入: 701
输出: "ZY"
</td></tr></table>
```
/**
 * @param {number} n
 * @return {string}
 */
let arr = ['A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z'];
var convertToTitle = function(n) {
    if(n === 0 ) return 'A';
    if(n <= 26)  return arr[n-1]
    let num = Math.floor(n/26);
    let num1 = n%26;
    let str = num1 ? arr[num1 - 1] : 'Z';
    num = num1 ? num : (num-1);
    return convertToTitle(num) + str;
};
```
## 38. 多数元素
给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: [3,2,3]
输出: 3
🌰：输入: [2,2,1,1,1,2,2]
输出: 2
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var majorityElement = function(nums) {
    let n=nums[0],count=0;
    for(let i=1;i<nums.length;i++){
        if(nums[i] === n) {
            count++; 
            continue;
        }
        count === 0 ? n = nums[i] : count--;
    }  
    return  n;
};
```
## 39. Excel表列序号
给定一个Excel表格中的列名称，返回其相应的列序号。
例如，

    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
<table><tr><td bgcolor=#D1EEEE>🌰：输入: "A"
输出: 1
🌰：输入: "AB"
输出: 28
🌰：输入: "ZY"
输出: 701
</td></tr></table>
```
/**
 * @param {string} s
 * @return {number}
 */
var titleToNumber = function(s) {
   let obj = {'A':1,'B':2,'C':3,'D':4,'E':5,'F':6,'G':7,'H':8,'I':9,'J':10,'K':11,'L':12,'M':13,'N':14,'O':15,'P':16,'Q':17,'R':18,'S':19,'T':20,'U':21,'V':22,'W':23,'X':24,'Y':25,'Z':26}; 
   if(s.length === 1){
       return obj[s];
   }
   let arr = s.split('');
   let sum = 0;
   let x = arr.length-1;
   let y = 0;
   while(x >=0 && y < arr.length){
        sum = sum + Math.pow(26, x) * obj[arr[y]]
        x--;
        y++;
   }
   return sum;
};
```
## 40. 阶乘后的零
给定一个整数 n，返回 n! 结果尾数中零的数量。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 3
输出: 0
解释: 3! = 6, 尾数中没有零。
🌰：输入: 5
输出: 1
解释: 5! = 120, 尾数中有 1 个零.
</td></tr></table>
```
/**
 * @param {number} n
 * @return {number}
 * 基本思路：查找该数是5的几倍，以及能够整除5几次；
 * 例 31 = 5 + 2*5 + 3*5 + 4*5 + 5*5 + 6*5；
 */
var trailingZeroes = function(n) {
    let count = 0;
    while(n >= 5){
        let j = Math.floor(n / 5);
        count = count + j;
        n = j;
    }
    return count;
};
```
## 41. 存在重复元素 II
给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引 i 和 j，使得 nums [i] = nums [j]，并且 i 和 j 的差的 绝对值 不超过为 k。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: nums = [1,0,1,1], k = 1
输出: true
🌰：输入: nums = [1,2,3,1], k = 3
输出: true
🌰：输入: nums = [1,2,3,1,2,3], k = 2
输出: false
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {boolean}
 */
var containsNearbyDuplicate = function(nums, k) {
    if(nums.length < 1) return false;
    for(let i = 0; i < nums.length; i++){
        for(let j = i+1; j <= i+k; j++){
            if(nums[i] === nums[j] ) return true;
        }
    }
    return false;
};
(方法不是最优解，目前想到这种解法)
```
## 42. 翻转二叉树
翻转一棵二叉树。
<table><tr><td bgcolor=#D1EEEE>🌰：
```
输入：           输出：
     4               4
   /   \           /   \
  2     7         7     2
 / \   / \       / \   / \
1   3 6   9     9   6 3   1
```
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function(root) {
    if(!root) return null;
    let right = invertTree(root.right);
    let left = invertTree(root.left);
    root.left = right;
    root.right = left;
    return root;
}
```
## 43. 2的幂
给定一个整数，编写一个函数来判断它是否是 2 的幂次方。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 1
输出: true
解释: 20 = 1
🌰：输入: 16
输出: true
解释: 24 = 16
🌰：输入: 218
输出: false
</td></tr></table>
```
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfTwo = function(n) {
    if(n < 0) return false;
    let i = 0;
    while(Math.pow(2,i) <= n){
        if(Math.pow(2,i) === n) return true;
        i++;
    }
    return false;
};
```
## 44. 请判断一个链表是否为回文链表。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 1->2
输出: false
🌰：输入: 1->2->2->1
输出: true
</td></tr></table>
```
/**
 * @param {ListNode} head
 * @return {boolean}
 * 思路：正序以及倒序是完全相等的，则是回文链表
 */
var isPalindrome = function(head) {
    let arr = []
    let arr1 = []
    let node = head
    while(node){
        arr.push(node.val)
        arr1.unshift(node.val)
        node = node.next
    }
    return JSON.stringify(arr) === JSON.stringify(arr1)
};
```
## 45. 有效的字母异位词。
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: s = "anagram", t = "nagaram"
输出: true
🌰：输入: s = "rat", t = "car"
输出: false
</td></tr></table>
```
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isAnagram = function(s, t) {
    return s.split('').sort().join('') == t.split('').sort().join('')
};
```
## 46. 二叉树的所有路径
给定一个二叉树，返回所有从根节点到叶子节点的路径。
说明: 叶子节点是指没有子节点的节点。
<table><tr><td bgcolor=#D1EEEE>🌰：输入:
```
   1
 /   \
2     3
 \
  5
```
输出: ["1->2->5", "1->3"]
解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
</td></tr></table>
```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {string[]}
 */
var binaryTreePaths = function(root) {
    const routeSum = (node, curString) => {
        let newString = curString ? curString + '->' + node.val : node.val + ''
        if(!node.left && !node.right) {resArr.push(newString)}
        else{
            if(node.left) routeSum(node.left, newString)
            if(node.right) routeSum(node.right, newString)
        }
    }

    let resArr = []
    if(root) routeSum(root, null)
    return resArr
};
```
## 47. 各位相加
给定一个非负整数 num，反复将各个位上的数字相加，直到结果为一位数。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 38
输出: 2 
解释: 各位相加的过程为：3 + 8 = 11, 1 + 1 = 2。 由于 2 是一位数，所以返回 2。
</td></tr></table>
```
/**
 * @param {number} num
 * @return {number}
 */
(方法一：基础方法)
const addSum = (num) => {
    let str = num + ''
    let sum = 0;
    for(let i of str){
        sum = sum + Number(i)
    }
    return sum
}
var addDigits = function(num) {
    if(num < 10) return num
    let sum = addSum(num)
    while(sum >= 10){
        sum = addSum(sum)
    }
    return  sum 
};


(方法二：数学法)
var addDigits = function(num) {
    if (num < 10) return num
    return num % 9 || 9
};
```
## 48. 丑数
编写一个程序判断给定的数是否为丑数。
丑数就是只包含质因数 2, 3, 5 的正整数。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 6
输出: true
解释: 6 = 2 × 3
🌰：输入: 8                   
输出: true                   
解释: 8 = 2 × 2 × 2          
🌰：输入: 14
输出: false 
解释: 14 不是丑数，因为它包含了另外一个质因数 7。
</td></tr></table>
```
/**
 * @param {number} num
 * @return {boolean}
 */
var isUgly = function(num) {
    if(num < 1) return false
    let res = num;
    while(res % 2 === 0) res = res / 2
    while(res % 3 === 0) res = res / 3
    while(res % 5 === 0) res = res / 5

    return res === 1
};
```
## 49. 缺失数字
给定一个包含 0, 1, 2, ..., n 中 n 个数的序列，找出 0 .. n 中没有出现在序列中的那个数。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: [3,0,1]
输出: 2        
🌰：输入: [9,6,4,2,3,5,7,0,1]
输出: 8
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var missingNumber = function(nums) {
    if(!nums.length) return 0
    for(let i = 0; i<= nums.length; i++){
        if(nums.indexOf(i) === -1) return i
    }
};
```
## 50. 移动零
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
说明:
必须在原数组上操作，不能拷贝额外的数组。
尽量减少操作次数。
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function(nums) {
    if(!nums.length) return nums

    for(let i = 0; i<nums.length; i++){
        if(nums[i] === 0){
            let item = nums.splice(i, 1)
            nums.push(item)
            i--;
        }
    }
    return nums
};
```