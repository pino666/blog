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
## 19. 数字的补数
给定一个正整数，输出它的补数。补数是对该数的二进制表示取反。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 5
输出: 2
解释: 5 的二进制表示为 101（没有前导零位），其补数为 010。所以你需要输出 2 。
🌰：输入: 1
输出: 0
解释: 1 的二进制表示为 1（没有前导零位），其补数为 0。所以你需要输出 0 。
</td></tr></table>
```
/**
 * @param {number} num
 * @return {number}
 */
var findComplement = function(num) {
    const num1 = parseInt(num).toString(2).split('')
    const num2 = num1.map(item => item === '1' ? '0' : '1').join('')
    return parseInt(num2, 2)

};
```
## 20. 密钥格式化
有一个密钥字符串 S ，只包含字母，数字以及 '-'（破折号）。其中， N 个 '-' 将字符串分成了 N+1 组。
给你一个数字 K，请你重新格式化字符串，除了第一个分组以外，每个分组要包含 K 个字符；而第一个分组中，至少要包含 1 个字符。两个分组之间需要用 '-'（破折号）隔开，并且将所有的小写字母转换为大写字母。
给定非空字符串 S 和数字 K，按照上面描述的规则进行格式化。
<table><tr><td bgcolor=#D1EEEE>🌰：输入：S = "5F3Z-2e-9-w", K = 4
输出："5F3Z-2E9W"
解释：字符串 S 被分成了两个部分，每部分 4 个字符；
     注意，两个额外的破折号需要删掉。
🌰：输入：S = "2-5g-3-J", K = 2
输出："2-5G-3J"
解释：字符串 S 被分成了 3 个部分，按照前面的规则描述，第一部分的字符可以少于给定的数量，其余部分皆为 2 个字符。
提示:
    S 的长度可能很长，请按需分配大小。K 为正整数。
    S 只包含字母数字（a-z，A-Z，0-9）以及破折号'-'
    S 非空
</td></tr></table>
```
/**
 * @param {string} S
 * @param {number} K
 * @return {string}
 */
var licenseKeyFormatting = function(S, K) {
    let str = S.toUpperCase().replace(/\-/g,'');
    let index = str.length % K;
    let str1 = str.slice(0, index)
    let str2 = str.slice(index)
    let res = [];
    let count = str2.length / K;
    for(let i = 0; i < count; i++) {
        res.push(str2.substr(i * K, K))
    }
    if (str1 !== '') {
        res.unshift(str1)
    }
    return res.join('-');
};
||
var licenseKeyFormatting = function(S, K) {
    let str = S.toUpperCase().replace(/\-/g,'');
    let i = str.length - K;
    let res = ''
    while(i + K > 0){
        res = (i > 0 ? '-' : '') + str.substring(i, i+K) + res;
        i -= K;
    }
    return res;
};
```
## 21. 最大连续1的个数
给定一个二进制数组， 计算其中最大连续1的个数。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: [1,1,0,1,1,1]
输出: 3
解释: 开头的两位和最后的三位都是连续1，所以最大连续1的个数是 3.
注意：
    输入的数组只包含 0 和1。
    输入数组的长度是正整数，且不超过 10,000。
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMaxConsecutiveOnes = function(nums) {
    if(!nums.length) return 0
    let num = 0 , count  = 0
    for(let i of nums){
        if(!i) {
            count = count > num ? count : num
            num = 0
            continue;
        }
        num++;
    }
    return count > num ? count : num;
};
```
## 22. 构造矩形
作为一位web开发者， 懂得怎样去规划一个页面的尺寸是很重要的。 现给定一个具体的矩形页面面积，你的任务是设计一个长度为 L 和宽度为 W 且满足以下要求的矩形的页面。要求：
1. 你设计的矩形页面必须等于给定的目标面积。
2. 宽度 W 不应大于长度 L，换言之，要求 L >= W 。
3. 长度 L 和宽度 W 之间的差距应当尽可能小。
你需要按顺序输出你设计的页面的长度 L 和宽度 W。
说明:
    给定的面积不大于 10,000,000 且为正整数。
    你设计的页面的长度和宽度必须都是正整数。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 4
输出: [2, 2]
解释: 目标面积是 4， 所有可能的构造方案有 [1,4], [2,2], [4,1]。
但是根据要求2，[1,4] 不符合要求; 根据要求3，[2,2] 比 [4,1] 更能符合要求. 所以输出长度 L 为 2， 宽度 W 为 2。
</td></tr></table>
```
/**
 * @param {number} area
 * @return {number[]}
 */
var constructRectangle = function(area) {
    let w = Math.floor(Math.sqrt(area))
    while(area%w){
        w--;
    }
    return [area/w, w]
};
```
## 23. 下一个更大元素 I
给定两个 没有重复元素 的数组 nums1 和 nums2 ，其中nums1 是 nums2 的子集。找到 nums1 中每个元素在 nums2 中的下一个比其大的值。
nums1 中数字 x 的下一个更大元素是指 x 在 nums2 中对应位置的右边的第一个比 x 大的元素。如果不存在，对应位置输出 -1 。
提示：
    nums1和nums2中所有元素是唯一的。
    nums1和nums2 的数组大小都不超过1000。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: nums1 = [4,1,2], nums2 = [1,3,4,2].
输出: [-1,3,-1]
解释:
    对于num1中的数字4，你无法在第二个数组中找到下一个更大的数字，因此输出 -1。
    对于num1中的数字1，第二个数组中数字1右边的下一个较大数字是 3。
    对于num1中的数字2，第二个数组中没有下一个更大的数字，因此输出 -1。
🌰：输入: nums1 = [2,4], nums2 = [1,2,3,4].
输出: [3,-1]
解释:
    对于 num1 中的数字 2 ，第二个数组中的下一个较大数字是 3 。
    对于 num1 中的数字 4 ，第二个数组中没有下一个更大的数字，因此输出 -1 。
```
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */

var searchIndex = (arr, num, index) => {
    const arr1  = arr.slice(index)
    for(let i of arr1){
        if(i > num) return i
    }
    return -1
}
var nextGreaterElement = function(nums1, nums2) {
    let max = Math.max(nums2)
    let res = []
    for(let j of nums1){
        if(j > max) {
            res.push(-1)
        }else{
            const num = searchIndex(nums2, j, nums2.indexOf(j)+1)
            res.push(num)
        }
    }
    return res
};
||
var nextGreaterElement = function(nums1, nums2) {
    let res = [];
    for(let i = 0; i < nums1.length; i++){
        let num = nums2.indexOf(nums1[i]);
        let findNum = nums2.findIndex((item,index) => item > nums1[i] && index > num);
        findNum !== -1 ? res.push(nums2[findNum]) : res.push(-1)
    }
    return res;
};
```
## 24. 二叉搜索树中的众数
给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。
假定 BST 有如下定义：
    结点左子树中所含结点的值小于等于当前结点的值
    结点右子树中所含结点的值大于等于当前结点的值
    左子树和右子树都是二叉搜索树
提示：如果众数超过1个，不需考虑输出顺序
<table><tr><td bgcolor=#D1EEEE>🌰：给定 BST [1,null,2,2],
   1
    \
     2
    /
   2
返回[2].
</td></tr></table>
```
//**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var findMode = function(root) {
    let res = [], maxCount = 0, count = 0, preVal = null;
    let search = (node) => {
        if(node === null) return;
        search(node.left)
        if(node.val === preVal){
            count++;
        }else{
            count = 1;
        }
        if(maxCount === count){
            res.push(node.val)
        }else if(maxCount < count){
            res = [node.val]
            maxCount = count
        }
        preVal = node.val
        search(node.right) 
    }
    search(root);
    return res;
};
```
## 25. 七进制数
给定一个整数，将其转化为7进制，并以字符串形式输出。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 100
输出: "202"
解释: 5 的二进制表示为 101（没有前导零位），其补数为 010。所以你需要输出 2 。
🌰：输入: -7
输出: "-10"
</td></tr></table>
```
/**
 * @param {number} num
 * @return {string}
 */
var convertToBase7 = function(num) {
    if(!num) return '0'
    let res = []
    const transform = (data) => {
        while(data >= 7){
            res.unshift(data % 7)
            data = Math.floor(data / 7)
        }
        res.unshift(data)
    }
    num > 0 ? transform(num) : transform(Math.abs(num))
    return num > 0 ? res.join('') : '-' + res.join('')
};
```
## 26. 相对名次
给出 N 名运动员的成绩，找出他们的相对名次并授予前三名对应的奖牌。前三名运动员将会被分别授予 “金牌”，“银牌” 和“ 铜牌”（"Gold Medal", "Silver Medal", "Bronze Medal"）。
(注：分数越高的选手，排名越靠前。)
<table><tr><td bgcolor=#D1EEEE>🌰：输入: [5, 4, 3, 2, 1]
输出: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
解释: 前三名运动员的成绩为前三高的，因此将会分别被授予 “金牌”，“银牌”和“铜牌” ("Gold Medal", "Silver Medal" and "Bronze Medal").
余下的两名运动员，我们只需要通过他们的成绩计算将其相对名次即可。
提示:
    N 是一个正整数并且不会超过 10000。
    所有运动员的成绩都不相同。
</td></tr></table>
```
/**
 * @param {number[]} nums
 * @return {string[]}
 */
var findRelativeRanks = function(nums) {
    let arr = nums.map(item => item)
    arr.sort((a,b) => b-a)
    let res = []
    let obj = {
        0:'Gold Medal',
        1:'Silver Medal',
        2:'Bronze Medal',
    }
    let i = 0
    while(i< nums.length){
        if(arr.indexOf(nums[i]) < 3){
            res.push(obj[arr.indexOf(nums[i])])
        }else{
            res.push((arr.indexOf(nums[i])+1) + '')
        }  
        i++
    }
    return res
};
||
var findRelativeRanks = function(nums) {
    const arr = [...nums].sort((a, b) => b - a)
    const map = new Map()
    arr.forEach((item, index) => {
        map.set(item, `${index + 1}`)
    })
    return nums.map(i => {
        if (map.get(i) === '1') return 'Gold Medal'
        if (map.get(i) === '2') return  "Silver Medal"
        if (map.get(i) === '3') return  "Bronze Medal"
        return map.get(i)
    })
};
```
## 27. 完美数
对于一个 正整数，如果它和除了它自身以外的所有正因子之和相等，我们称它为“完美数”。
给定一个 整数 n， 如果他是完美数，返回 True，否则返回 False
<table><tr><td bgcolor=#D1EEEE>🌰：输入: 28
输出: True
解释: 28 = 1 + 2 + 4 + 7 + 14
</td></tr></table>
```
/**
 * @param {number} num
 * @return {boolean}
 */
var checkPerfectNumber = function(num) {
    if(num === 1) return false
    let sum = 1
    for(let i = 2; i <= Math.sqrt(num);i++){
        if(num % i === 0){
            sum +=i
            if(Math.pow(i, 2) !== num){
                sum += num/i
            }
        }
    }
    return sum === num
};
```
## 28. 斐波那契数
斐波那契数，通常用 F(n) 表示，形成的序列称为斐波那契数列。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是
    F(0) = 0,   F(1) = 1
    F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
给定 N，计算 F(N)。
<table><tr><td bgcolor=#D1EEEE>🌰：输入：2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1.
🌰：输入：3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2.
🌰：输入：4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3.
</td></tr></table>
```
/**
 * @param {number} N
 * @return {number}
 */
// 递归
var fib = function(N) {
  if(N === 0 || N === 1) return N
  return fib(N-1) + fib(N-2) 
};
||
// 动态规划
var fib = function(N) {
    if(N === 0) return 0
    if(N === 1 || N === 2) return 1
    let pre = 1
    let curr = 1
    for(let i = 3; i <= N; i++){
        let sum = pre + curr
        pre = curr
        curr = sum
    }
    return curr 
};
```
## 29. 最长特殊序列 Ⅰ
给你两个字符串，请你从这两个字符串中找出最长的特殊序列。
「最长特殊序列」定义如下：该序列为某字符串独有的最长子序列（即不能是其他字符串的子序列）。
子序列 可以通过删去字符串中的某些字符实现，但不能改变剩余字符的相对顺序。空序列为所有字符串的子序列，任何字符串为其自身的子序列。
输入为两个字符串，输出最长特殊序列的长度。如果不存在，则返回 -1。
<table><tr><td bgcolor=#D1EEEE>🌰：输入: "aba", "cdc"
输出: 3
解释: 最长特殊序列可为 "aba" (或 "cdc")，两者均为自身的子序列且不是对方的子序列
🌰：输入：a = "aaa", b = "aaa"
输出：-1
</td></tr></table>
```
/**
 * @param {string} a
 * @param {string} b
 * @return {number}
 */
var findLUSlength = function(a, b) {
    if(a === b) return -1
    return a.length > b.length ? a.length : b.length
};
```
## 30. 二叉搜索树的最小绝对差
给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值
<table><tr><td bgcolor=#D1EEEE>🌰：输入
   1
    \
     3
    /
   2
输出：1
解释：最小绝对差为 1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。
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
var getMinimumDifference = function(root) {
    let pre = null
    let min = Infinity
    let middleErgodic = (root) => {
        if(!root) return
        middleErgodic(root.left)
        if(pre) {
            min = min < Math.abs(pre.val - root.val) ? min : Math.abs(pre.val - root.val)
        }
        pre = root
        middleErgodic(root.right)
    }
    middleErgodic(root)
    return min
};
```
