---
title: leetcode刷题小记二(简单版)
date: 2020-05-11 15:44:08
tags: -JavaScript
---
{% blockquote %}
本文参考于题目出自leetcode官方，有些答案参考于网络，仅供学习使用～
{% endblockquote %}

## 1. 二叉搜索树的最近公共祖先
给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
<table><tr><td bgcolor=#D1EEEE> 给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]
```
         6
       /   \
      2     8
     / \   / \
    0   4 7   9
       / \
      3   5
```
🌰：输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
🌰：输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function(root, p, q) {
    if(p.val < root.val && root.val > q.val){
       return lowestCommonAncestor(root.left, p, q);
    }
    if(p.val > root.val && root.val < q.val){
       return lowestCommonAncestor(root.right, p, q);
    }
    return root;
};
```
## 2. 单词规律
给定一种规律 pattern 和一个字符串 str ，判断 str 是否遵循相同的规律。
这里的 遵循 指完全匹配，例如， pattern 里的每个字母和字符串 str 中的每个非空单词之间存在着双向连接的对应规律。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: pattern = "abba", str = "dog cat cat dog"
输出: true
🌰：输入:pattern = "abba", str = "dog cat cat fish"
输出: false
🌰：输入: pattern = "aaaa", str = "dog cat cat dog"
输出: false
🌰：输入: pattern = "abba", str = "dog dog dog dog"
输出: false
说明:
你可以假设 pattern 只包含小写字母， str 包含了由单个空格分隔的小写字母。
</td></tr></table>
```
/**
 * @param {string} pattern
 * @param {string} str
 * @return {boolean}
 */
var wordPattern = function(pattern, str) {
    let strArr = str.split(' ')
    if(pattern.length !== strArr.length) return false
    for(let i = 0; i< pattern.length; i++){
        if(pattern.indexOf(pattern[i]) !== strArr.indexOf(strArr[i])){
            return false
        } 
    }
    return true
};
```
## 3. Nim 游戏
你和你的朋友，两个人一起玩 Nim 游戏：桌子上有一堆石头，每次你们轮流拿掉 1 - 3 块石头。 拿掉最后一块石头的人就是获胜者。你作为先手。

你们是聪明人，每一步都是最优解。 编写一个函数，来判断你是否可以在给定石头数量的情况下赢得游戏。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 4
输出: false 
解释: 如果堆中有 4 块石头，那么你永远不会赢得比赛；
     因为无论你拿走 1 块、2 块 还是 3 块石头，最后一块石头总是会被你的朋友拿走。
</td></tr></table>
```
/**
 * @param {number} n
 * @return {boolean}
 */
var canWinNim = function(n) {
    return n % 4
};
（解释：当石头数为1，2，3，4时，知道只有当4个石头的时候，
我没有办法赢下比赛，那么就可以考虑多数石头的情况下，我可否先手后让朋友拿到4个石头；
推理发现，当石头数为的倍数时，都无法通过最优解取得胜利。）
```
## 4. 3的幂
给定一个整数，写一个函数来判断它是否是 3 的幂次方。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 27
输出: true
🌰：输入: 0
输出: false
🌰：输入: 9
输出: true
</td></tr></table>
```
/**
 * @param {number} n
 * @return {boolean}
 */
var isPowerOfThree = function(n) {
    if(n < 1) return false
    let res = 0
    for(let i = 0;;i++){
        let x = Math.pow(3,i)
        if(x === n) return true
        if(x > n || n % x !== 0) return false
    }
};
||
var isPowerOfThree = function(n) {
   return /^10*$/.test(n.toString(3));
};
(解释：将数字转化为三进制字符串，并用正则表达式匹配，同样适用于别的数字)
```
## 5. 反转字符串
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。
不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。
你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符
<table><tr><td bgcolor=#D1EEEE>🌰：输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
🌰：输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
</td></tr></table>
```
/**
 * @param {character[]} s
 * @return {void} Do not return anything, modify s in-place instead.
 */
var reverseString = function(s) {
    return s.reverse()
};
||
var reverseString = function(s) {
    let i = 0; j = s.length - 1
    while(i < j){
        [s[i], s[j]] = [s[j], s[i]];
        i++;j-- 
    }
    return s
};
(利用ES6的解构赋值进行两数交换)
```
## 6. 反转字符串中的元音字母
编写一个函数，以字符串作为输入，反转该字符串中的元音字母。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: "hello"
输出: "holle"
🌰：输入: "leetcode"
输出: "leotcede"
说明:
元音字母不包含字母"y"。
</td></tr></table>
```
/**
 * @param {string} s
 * @return {string}
 */
var reverseVowels = function(s) {
    let setArr = new Set(['a','e','i','o','u','A','E','I','O','U'])
    let i = 0; j = s.length;
    let arr = s.split('')

    while(i < j){
        if(setArr.has(arr[i])){
            if(setArr.has(arr[j])){
                [arr[i], arr[j]] = [arr[j], arr[i]]
                i++;
            }
            j--;
        }else{
            i++;
        }
    }
    return arr.join('')

};
```
## 7. 两个数组的交集
给定两个数组，编写一个函数来计算它们的交集。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2]
🌰：输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [9,4]
</td></tr></table>
```
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function(nums1, nums2) {
    let setArr = new Set([])

    for(let item of nums1){
        if(nums2.indexOf(item) !== -1) setArr.add(item)
    }
    return [...setArr]
};
```
## 8. 有效的完全平方数
给定一个正整数 num，编写一个函数，如果 num 是一个完全平方数，则返回 True，否则返回 False。
说明：不要使用任何内置的库函数，如  sqrt。
<table><tr><td bgcolor=#D1EEEE>🌰：输入：16
输出：True
🌰：输入：14
输出：False
</td></tr></table>
```
/**
 * @param {number} num
 * @return {boolean}
 */
var isPerfectSquare = function(num) {
    if(num < 1) return false
    for(let i = 0; ; i++){
        let x = i * i
        if(x === num) return true
        if(x > num) return false
    }
};
```