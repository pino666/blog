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
## 9. 猜数字大小
我们正在玩一个猜数字游戏。 游戏规则如下：
我从 1 到 n 选择一个数字。 你需要猜我选择了哪个数字。
每次你猜错了，我会告诉你这个数字是大了还是小了。
你调用一个预先定义好的接口 guess(int num)，它会返回 3 个可能的结果（-1，1 或 0）：
-1 : 我的数字比较小
 1 : 我的数字比较大
 0 : 恭喜！你猜对了！
<table><tr><td bgcolor=#D1EEEE>🌰：输入: n = 10, pick = 6
输出: 6
</td></tr></table>
```
/**
 * @param {number} n
 * @return {number}
 */
var guessNumber = function(n) {
    let left = 1
    let right = n
    let res = 0

    while(left <= right){
        res = Math.floor((left + right) / 2)
        const status = guess(res)
        if(status === 0) break;
        if(status === 1) {
            left = res + 1
        }
        if(status === -1) {
            right = res - 1
        }
    }
    return res
};
```
## 10. 赎金信
给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串 ransom 能不能由第二个字符串 magazines 里面的字符构成。如果可以构成，返回 true ；否则返回 false。

(题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。杂志字符串中的每个字符只能在赎金信字符串中使用一次。)
<table><tr><td bgcolor=#D1EEEE>注意：你可以假设两个字符串均只含有小写字母。
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
</td></tr></table>
```
/**
 * @param {string} ransomNote
 * @param {string} magazine
 * @return {boolean}
 */
var canConstruct = function(ransomNote, magazine) {
    let str = '';
    let magazineArr = magazine.split('')
    for(item of ransomNote){
        const index = magazineArr.indexOf(item)
        if(index !== -1){
            const i = magazineArr.splice(index, 1)
            str += i
        }
    }
    return str === ransomNote
};
```
## 11. 字符串中的第一个唯一字符
给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。
<table><tr><td bgcolor=#D1EEEE>🌰：s = "leetcode"
返回 0.
s = "loveleetcode",
返回 2.
</td></tr></table>
```
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
    for(let i = 0; i< s.length; i++){
        if(s.indexOf(s[i]) === s.lastIndexOf(s[i])) return i
    }
    return  -1
};
```
## 12. 找不同
给定两个字符串 s 和 t，它们只包含小写字母。
字符串 t 由字符串 s 随机重排，然后在随机位置添加一个字母。
请找出在 t 中被添加的字母
<table><tr><td bgcolor=#D1EEEE>🌰：输入：
s = "abcd"
t = "abcde"
输出：
e
解释：
'e' 是那个被添加的字母
</td></tr></table>
```
/**
 * @param {string} s
 * @param {string} t
 * @return {character}
 */
var findTheDifference = function(s, t) {
    // 取巧方法， 改变了原数据
  for(let item of s){
    // 其中replace方法第一个参数包含 /g 的话，
    // 会全局查找，替换所有符合条件单词，否则替换第一个。
    t = t.replace(item, '')
  }
  return t
};
```
## 13. 判断子序列
给定字符串 s 和 t ，判断 s 是否为 t 的子序列。
你可以认为 s 和 t 中仅包含英文小写字母。字符串 t 可能会很长（长度 ~= 500,000），而 s 是个短字符串（长度 <=100）。
字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。
<table><tr><td bgcolor=#D1EEEE>🌰：示例 1:
s = "abc", t = "ahbgdc"
返回 true.
示例 2:
s = "axc", t = "ahbgdc"
返回 false.
</td></tr></table>
```
/**
 * @param {string} s
 * @param {string} t
 * @return {boolean}
 */
var isSubsequence = function(s, t) {
    if(!s.length) return true
    let sIndex = 0;
    let tIndex = 0;

    while(tIndex < t.length){
        if(s[sIndex] === t[tIndex]) sIndex++
        if( sIndex === s.length) return true
        tIndex++
    }
    return false
};
```
## 14. 左叶子之和
计算给定二叉树的所有左叶子之和。
<table><tr><td bgcolor=#D1EEEE>🌰：
    3
   / \
  9  20
    /  \
   15   7
在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
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
var sumOfLeftLeaves = function(root) {
    if(!root) return null;
    let sum = 0
    if(root.left && !root.left.left && !root.left.right){
        sum = root.left.val
    }
    return sum + sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right)
};
```
## 15. 第三大的数
给定一个非空数组，返回此数组中第三大的数。如果不存在，则返回数组中最大的数。要求算法时间复杂度必须是O(n)。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: [3, 2, 1]
输出: 1
解释: 第三大的数是 1.
🌰：输入: [1, 2]
输出: 2
解释: 第三大的数不存在, 所以返回最大的数 2 .
🌰：输入: [2, 2, 3, 1]
输出: 1
解释: 注意，要求返回第三大的数，是指第三大且唯一出现的数。存在两个值为2的数，它们都排第二。
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var thirdMax = function(nums) {
    if(nums.length < 3) return Math.max(...nums)

    let max1 = -Infinity
    let max2 = -Infinity
    let max3 = -Infinity

    for(let n of nums){
        if(n > max1){
            max3 = max2
            max2 = max1
            max1 = n
            continue
        }
        if(n !== max1 && n > max2){
            max3 = max2
            max2 = n
            continue
        }
        if(n !== max1 && n !== max2 && n > max3){
            max3 = n
            continue
        }
    }
    if(max1 === -Infinity || max2 === -Infinity || max3 === -Infinity) return Math.max(max1, max2,max3)
    return max3
};
```
## 16. 字符串相加
给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。
<table><tr><td bgcolor=#D1EEEE>注意：
num1 和num2 的长度都小于 5100.
num1 和num2 都只包含数字 0-9.
num1 和num2 都不包含任何前导零。
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式。
</td></tr></table>
```
/**
 * @param {string} num1
 * @param {string} num2
 * @return {string}
 */
var addStrings = function(num1, num2) {
    let res = ''
    let i = num1.length - 1
    let j = num2.length - 1
    let flag = 0
    let sum = 0

    while(i >=0 || j >=0 || flag){
        let str1 = +num1[i--] || 0
        let str2 = +num2[j--] || 0
        sum = str1 + str2 + flag
        if(sum >= 10){
            flag = 1
            sum = sum - 10
        }else(
            flag = 0
        )
        res = sum + '' + res
    }

    return res

};
```
## 17. 字符串中的单词数
统计字符串中的单词个数，这里的单词指的是连续的不是空格的字符。
请注意，你可以假定字符串里不包括任何不可打印的字符。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: "Hello, my name is John"
输出: 5
解释: 这里的单词是指连续的不是空格的字符，所以 "Hello," 算作 1 个单词。
</td></tr></table>
```
/**
 * @param {string} s
 * @return {number}
 */
var countSegments = function(s) {
    if(!s) return 0
    let res = 0
    let flag = 1
    for(let n of s){
        if(n === ' ') {
            if(flag) continue
            flag = 1
            res +=1
        }else{
            flag = 0
        }
    }
    return flag ? res : res+1
};
||
var countSegments = function(s) {
    return s.length > 0 ? s.split(' ').filter(item => item.trim() !== '').length : 0
};
```
## 18. 排列硬币
你总共有 n 枚硬币，你需要将它们摆成一个阶梯形状，第 k 行就必须正好有 k 枚硬币。
给定一个数字 n，找出可形成完整阶梯行的总行数。
n 是一个非负整数，并且在32位有符号整型的范围内。
<table><tr><td bgcolor=#D1EEEE>🌰：n = 5
硬币可排列成以下几行:
¤
¤ ¤
¤ ¤
因为第三行不完整，所以返回2.
🌰：n = 8
硬币可排列成以下几行:
¤
¤ ¤
¤ ¤ ¤
¤ ¤
因为第四行不完整，所以返回3.
</td></tr></table>
```
/**
 * @param {number} n
 * @return {number}
 */
var arrangeCoins = function(n) {
    let i = 1
    let sum = 0;
    while(sum <= n){
        if(sum === n) return i - 1
        sum += i
        i++
    }
    return i - 2
};
```