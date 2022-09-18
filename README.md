# Quick-Hand-Algorithm

《快手-算法》 快手是面向普通人记录和分享生活的短视频社交平台

<img width="300px" src="https://user-images.githubusercontent.com/59645426/190641083-aafddd17-9f77-45f5-b707-0fe965abffca.png"/>

# [1\. 两数之和](https://leetcode.cn/problems/two-sum/)

## Description

Difficulty: **简单**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [哈希表](https://leetcode.cn/tag/hash-table/)


给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** _`target`_  的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

**提示：**

*   2 <= nums.length <= 10<sup>4</sup>
*   -10<sup>9</sup> <= nums[i] <= 10<sup>9</sup>
*   -10<sup>9</sup> <= target <= 10<sup>9</sup>
*   **只会存在一个有效答案**

**进阶：**你可以想出一个时间复杂度小于 O(n<sup>2</sup>) 的算法吗？


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    let map = new Map()
    for (let i = 0; i < nums.length; i++) {
        if (map.has(target - nums[i])) {
            return [map.get(target - nums[i]), i]
        }
        map.set(nums[i], i)
    }
    return [-1, -1]
}
```

# [2\. 两数相加](https://leetcode.cn/problems/add-two-numbers/)

## Description

Difficulty: **中等**  

Related Topics: [递归](https://leetcode.cn/tag/recursion/), [链表](https://leetcode.cn/tag/linked-list/), [数学](https://leetcode.cn/tag/math/)


给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例 1：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

**示例 2：**

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

**示例 3：**

```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

**提示：**

*   每个链表中的节点数在范围 `[1, 100]` 内
*   `0 <= Node.val <= 9`
*   题目数据保证列表表示的数字不含前导零


## Solution

Language: **JavaScript**

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    let dummy = new ListNode()
    let cur = dummy
    let carry = 0

    while(l1 || l2) {
        const x = l1 ? l1.val : 0
        const y = l2 ? l2.val : 0
        const sum = x + y + carry
        cur.next = new ListNode(sum % 10)
        cur = cur.next
        carry = Math.floor(sum / 10)

        if (l1) l1 = l1.next
        if (l2) l2 = l2.next
    }
    if (carry) cur.next = new ListNode(carry)

    return dummy.next
}
```

# [3\. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

## Description

Difficulty: **中等**  

Related Topics: [哈希表](https://leetcode.cn/tag/hash-table/), [字符串](https://leetcode.cn/tag/string/), [滑动窗口](https://leetcode.cn/tag/sliding-window/)


给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串 **的长度。

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

**提示：**

*   0 <= s.length <= 5 * 10<sup>4</sup>
*   `s` 由英文字母、数字、符号和空格组成


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} s
 * @return {number}
 */
// 双指针
var lengthOfLongestSubstring = function(s) {
    const set = new Set()
    let [l, r, maxLen] = [0, 0, 0]
    while (r < s.length) {
        set.has(s[r]) ? set.delete(s[l++]) : set.add(s[r++])
        maxLen = Math.max(maxLen, set.size)
    }
    return maxLen
}
```

# [4\. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)

## Description

Difficulty: **困难**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [二分查找](https://leetcode.cn/tag/binary-search/), [分治](https://leetcode.cn/tag/divide-and-conquer/)


给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**示例 2：**

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

**提示：**

*   `nums1.length == m`
*   `nums2.length == n`
*   `0 <= m <= 1000`
*   `0 <= n <= 1000`
*   `1 <= m + n <= 2000`
*   -10<sup>6</sup> <= nums1[i], nums2[i] <= 10<sup>6</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */

// 合并排序
var findMedianSortedArrays = function(nums1, nums2) {
    const newNums = nums1.concat(nums2).sort((a, b) => a - b)
    const len = newNums.length
    if (len % 2 === 0) {
        return (newNums[len/2 - 1] + newNums[len/2]) / 2
    } else {
        return newNums[Math.floor(len/2)]
    }
}
```

# [5\. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)

## Description

Difficulty: **中等**  

Related Topics: [字符串](https://leetcode.cn/tag/string/), [动态规划](https://leetcode.cn/tag/dynamic-programming/)


给你一个字符串 `s`，找到 `s` 中最长的回文子串。

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2：**

```
输入：s = "cbbd"
输出："bb"
```

**提示：**

*   `1 <= s.length <= 1000`
*   `s` 仅由数字和英文字母组成


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} s
 * @return {string}
 */
// 中心扩散
var longestPalindrome = function (s) {
    let max = ''
    for (let i = 0; i < s.length; i++) {
        helper(i, i)
        helper(i, i+1)
    }
    function helper(l, r) {
        while(l >= 0 && r < s.length && s[l] === s[r]) {
            l--
            r++
        }
        const maxStr = s.slice(l + 1, r - 1 + 1)
        if (maxStr.length > max.length) max = maxStr
    }
    return max
}
```

![image](https://user-images.githubusercontent.com/59645426/190664185-a8650b8c-dd4a-4c06-8dfa-dc66c468542e.png)

![image](https://user-images.githubusercontent.com/59645426/190664273-e7b366fd-e183-43a8-a70a-3e8dd57ab690.png)

![image](https://user-images.githubusercontent.com/59645426/190664306-aa3c240c-c2b9-4676-846e-3d097dcc3e8e.png)

![image](https://user-images.githubusercontent.com/59645426/190664321-c2289c93-b45e-4cd6-9729-21922eaedc06.png)

![image](https://user-images.githubusercontent.com/59645426/190664338-cf166477-6daa-471b-b9f8-2bcab16cd13d.png)

![image](https://user-images.githubusercontent.com/59645426/190664355-49f85e22-af01-4bc6-b04c-859f8fd5af80.png)

![image](https://user-images.githubusercontent.com/59645426/190664372-ecd8b430-66b8-4fc0-8147-89854ac3da07.png)

![image](https://user-images.githubusercontent.com/59645426/190664390-3cc8e5f7-ae59-4a18-ac14-f3e19d287f43.png)

![image](https://user-images.githubusercontent.com/59645426/190664413-01c5816e-c975-4494-975e-625ca2af5831.png)

# [6\. Z 字形变换](https://leetcode.cn/problems/zigzag-conversion/)

## Description

Difficulty: **中等**  

Related Topics: [字符串](https://leetcode.cn/tag/string/)


将一个给定字符串 `s` 根据给定的行数 `numRows` ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"PAYPALISHIRING"` 行数为 `3` 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"PAHNAPLSIIGYIR"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

**示例 1：**

```
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```

**示例 2：**

```
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

**示例 3：**

```
输入：s = "A", numRows = 1
输出："A"
```

**提示：**

*   `1 <= s.length <= 1000`
*   `s` 由英文字母（小写和大写）、`','` 和 `'.'` 组成
*   `1 <= numRows <= 1000`


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} s
 * @param {number} numRows
 * @return {string}
 */
var convert = function(s, numRows) {
    if (numRows === 1) return s;
    const len = Math.min(s.length, numRows);
    const rows = [];
    for (let i = 0; i < len; i++) rows[i] = '';
    let loc = 0;
    let down = false;

    for (const c of s) {
        rows[loc] += c
        if (loc === 0 || loc === numRows - 1) {
            down = !down
        }
        loc += down ? 1 : -1;
    }

    let ans = '';
    for (const row of rows) {
        ans += row;
    }
    return ans;
};
```

# [7\. 整数反转](https://leetcode.cn/problems/reverse-integer/)

## Description

Difficulty: **中等**  

Related Topics: [数学](https://leetcode.cn/tag/math/)


给你一个 32 位的有符号整数 `x` ，返回将 `x` 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 [−2<sup>31</sup>,  2<sup>31 </sup>− 1] ，就返回 0。

**假设环境不允许存储 64 位整数（有符号或无符号）。**

**示例 1：**

```
输入：x = 123
输出：321
```

**示例 2：**

```
输入：x = -123
输出：-321
```

**示例 3：**

```
输入：x = 120
输出：21
```

**示例 4：**

```
输入：x = 0
输出：0
```

**提示：**

*   -2<sup>31</sup> <= x <= 2<sup>31</sup> - 1


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number} x
 * @return {number}
 */
// | 0 移除小数点后的部分
var reverse = function(x) {
    let result = 0;
    while (x) {
        result = result * 10 + x % 10
        x = (x / 10) | 0
    }
    return (result | 0) === result ? result : 0
}
```

# [9\. 回文数](https://leetcode.cn/problems/palindrome-number/)

## Description

Difficulty: **简单**  

Related Topics: [数学](https://leetcode.cn/tag/math/)


给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

*   例如，`121` 是回文，而 `123` 不是。

**示例 1：**

```
输入：x = 121
输出：true
```

**示例 2：**

```
输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3：**

```
输入：x = 10
输出：false
解释：从右向左读, 为 01 。因此它不是一个回文数。
```

**提示：**

*   -2<sup>31</sup> <= x <= 2<sup>31</sup> - 1

**进阶：**你能不将整数转为字符串来解决这个问题吗？


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function(x) {
    return +x.toString().split('').reverse().join('') === x;
}
```

# [10\. 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/)

## Description

Difficulty: **困难**  

Related Topics: [递归](https://leetcode.cn/tag/recursion/), [字符串](https://leetcode.cn/tag/string/), [动态规划](https://leetcode.cn/tag/dynamic-programming/)


给你一个字符串 `s` 和一个字符规律 `p`，请你来实现一个支持 `'.'` 和 `'*'` 的正则表达式匹配。

*   `'.'` 匹配任意单个字符
*   `'*'` 匹配零个或多个前面的那一个元素

所谓匹配，是要涵盖 **整个 **字符串 `s`的，而不是部分字符串。

**示例 1：**

```
输入：s = "aa", p = "a"
输出：false
解释："a" 无法匹配 "aa" 整个字符串。
```

**示例 2:**

```
输入：s = "aa", p = "a*"
输出：true
解释：因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

**示例 3：**

```
输入：s = "ab", p = ".*"
输出：true
解释：".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```

**提示：**

*   `1 <= s.length <= 20`
*   `1 <= p.length <= 30`
*   `s` 只包含从 `a-z` 的小写字母。
*   `p` 只包含从 `a-z` 的小写字母，以及字符 `.` 和 `*`。
*   保证每次出现字符 `*` 时，前面都匹配到有效的字符


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} s
 * @param {string} p
 * @return {boolean}
 */
var isMatch = function(s, p) {
    p = new RegExp(p)
    s = s.replace(p, '')
    return s === '' ? true : false
}
```

# [11\. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)

## Description

Difficulty: **中等**  

Related Topics: [贪心](https://leetcode.cn/tag/greedy/), [数组](https://leetcode.cn/tag/array/), [双指针](https://leetcode.cn/tag/two-pointers/)


给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

**示例 1：**

![](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

**提示：**

*   `n == height.length`
*   2 <= n <= 10<sup>5</sup>
*   0 <= height[i] <= 10<sup>4</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
// 双指针
var maxArea = function(height) {
    let [l, r, ans] = [0, height.length - 1, 0]
    while(l < r) {
        let [hl, hr] = [height[l], height[r]]
        ans = Math.max(ans, Math.min(hl, hr) * (r - l))
        
        hl < hr ? l++ : r--
    }
    return ans
}
```

# [14\. 最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)

## Description

Difficulty: **简单**  

Related Topics: [字符串](https://leetcode.cn/tag/string/)


编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1：**

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

**示例 2：**

```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

**提示：**

*   `1 <= strs.length <= 200`
*   `0 <= strs[i].length <= 200`
*   `strs[i]` 仅由小写英文字母组成


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
// 获取第一个来比较
var longestCommonPrefix = function(strs) {
    let re = strs[0] || ''
    if (strs.length === 1) return strs[0]
    for (let i = 1; i < strs.length; i++) {
        while(strs[i].slice(0, re.length) !== re) {
            re = re.slice(0, re.length - 1)
        }
    }
    return re
}
```

# [15\. 三数之和](https://leetcode.cn/problems/3sum/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [双指针](https://leetcode.cn/tag/two-pointers/), [排序](https://leetcode.cn/tag/sorting/)


给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

**提示：**

*   `3 <= nums.length <= 3000`
*   -10<sup>5</sup> <= nums[i] <= 10<sup>5</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
// 三数之和为0 双指针 数组排序
var threeSum = function(nums) {
    let ans = []
    let len = nums.length
    nums.sort((a, b) => a - b)
    for (let i = 0; i < len - 1; i++) {
        if (nums[0] > 0) break
        if (i > 0 && nums[i] === nums[i-1]) continue
        let l = i + 1
        let r = len - 1
        while (l < r) {
            const sum = nums[i] + nums[l] + nums[r]
            if (sum === 0) {
                ans.push([nums[i], nums[l], nums[r]])
                while(l < r && nums[l] === nums[l+1]) l++
                while(l < r && nums[r] === nums[r-1]) r--
                l++
                r--
            } else if (sum < 0) {
                l++
            } else [
                r--
            ]
        }
    }
    return ans
}

```

# [17\. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

## Description

Difficulty: **中等**  

Related Topics: [哈希表](https://leetcode.cn/tag/hash-table/), [字符串](https://leetcode.cn/tag/string/), [回溯](https://leetcode.cn/tag/backtracking/)


给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png)

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = ""
输出：[]
```

**示例 3：**

```
输入：digits = "2"
输出：["a","b","c"]
```

**提示：**

*   `0 <= digits.length <= 4`
*   `digits[i]` 是范围 `['2', '9']` 的一个数字。


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} digits
 * @return {string[]}
 */
// 回溯 dfs(当前字符串''，下标0)
var letterCombinations = function (digits) {
    if (digits.length === 0) return []
    const res = []
    const map = ['', '', 'abc', 'def', 'ghi', 'jkl', 'mno', 'pqrs', 'tuv', 'wxyz']
    const dfs = (curStr, i) => {
        if (i > digits.length - 1) {
            res.push(curStr)
            return
        }
        const letters = map[digits[i]]
        for (let l of letters) {
            dfs(curStr + l, i + 1)
        }
    }
    dfs('', 0)
    return res
}
```

# [19\. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

## Description

Difficulty: **中等**  

Related Topics: [链表](https://leetcode.cn/tag/linked-list/), [双指针](https://leetcode.cn/tag/two-pointers/)


给你一个链表，删除链表的倒数第 `n`个结点，并且返回链表的头结点。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

**提示：**

*   链表中结点的数目为 `sz`
*   `1 <= sz <= 30`
*   `0 <= Node.val <= 100`
*   `1 <= n <= sz`

**进阶：**你能尝试使用一趟扫描实现吗？


## Solution

Language: **JavaScript**

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
// 快慢指针 new ListNode(-1, head) [dummy, dummy]
var removeNthFromEnd = function(head, n) {
    const dummy = new ListNode(-1, head)
    let [slow, fast] = [dummy, dummy]
    for (let i = 0; i <= n; i++) {
        fast = fast.next
    }
    while(fast) {
        slow = slow.next
        fast = fast.next
    }
    slow.next = slow.next.next
    return dummy.next
}
```

# [20\. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)

## Description

Difficulty: **简单**  

Related Topics: [栈](https://leetcode.cn/tag/stack/), [字符串](https://leetcode.cn/tag/string/)


给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1.  左括号必须用相同类型的右括号闭合。
2.  左括号必须以正确的顺序闭合。
3.  每个右括号都有一个对应的相同类型的左括号。

**示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

**提示：**

*   1 <= s.length <= 10<sup>4</sup>
*   `s` 仅由括号 `'()[]{}'` 组成


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
// 栈 true 栈为空
var isValid = function(s) {
    if (s.length % 2 !== 0) return false
    const map = {
        '(' : ')',
        '{' : '}',
        '[' : ']'
    }
    let stack = []
    for (let char of s) {
        if (map[char]) {
            stack.push(map[char])
        } else {
            let ret = stack.pop()
            if (ret !== char) {
                return false
            }
        }
    }
    return stack.length === 0
}

```

# [21\. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

## Description

Difficulty: **简单**  

Related Topics: [递归](https://leetcode.cn/tag/recursion/), [链表](https://leetcode.cn/tag/linked-list/)


将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例 2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例 3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```

**提示：**

*   两个链表的节点数目范围是 `[0, 50]`
*   `-100 <= Node.val <= 100`
*   `l1` 和 `l2` 均按 **非递减顺序** 排列


## Solution

Language: **JavaScript**

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} list1
 * @param {ListNode} list2
 * @return {ListNode}
 */
// 递归
// var mergeTwoLists = function(list1, list2) {
//     if (!list1) {
//         return list2
//     } else if (!list2) {
//         return list1
//     } else if (list1.val < list2.val) {
//         list1.next = mergeTwoLists(list1.next, list2)
//         return list1
//     } else {
//         list2.next = mergeTwoLists(list1, list2.next)
//         return list2
//     }
// }

// 迭代
var mergeTwoLists = function(list1, list2) {
    const dummy = new ListNode()
    let curr = dummy
    while (list1 && list2) {
        list1.val < list2.val
            ? [curr.next, list1] = [list1, list1.next]
            : [curr.next, list2] = [list2, list2.next]
        // curr节点指向下一个
        curr = curr.next
    }
    curr.next = list1 ?? list2
    return dummy.next
}
```

# [22\. 括号生成](https://leetcode.cn/problems/generate-parentheses/)

## Description

Difficulty: **中等**  

Related Topics: [字符串](https://leetcode.cn/tag/string/), [动态规划](https://leetcode.cn/tag/dynamic-programming/), [回溯](https://leetcode.cn/tag/backtracking/)


数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

**示例 1：**

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

**示例 2：**

```
输入：n = 1
输出：["()"]
```

**提示：**

*   `1 <= n <= 8`


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number} n
 * @return {string[]}
 */
// 回溯法(DFS) dfs(0, 0, '') dfs回溯 左边个数，右边个数，当前符合值
var generateParenthesis = function(n) {
    const res = []
    const dfs = (left, right, str) => {
        if (left === n && right === n) return res.push(str)
        if (left < n) dfs(left + 1, right, str + '(')
        if (right < left) dfs(left, right + 1, str + ')')
    }
    dfs(0, 0, '')
    return res
}
```

# [23\. 合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

## Description

Difficulty: **困难**  

Related Topics: [链表](https://leetcode.cn/tag/linked-list/), [分治](https://leetcode.cn/tag/divide-and-conquer/), [堆（优先队列）](https://leetcode.cn/tag/heap-priority-queue/), [归并排序](https://leetcode.cn/tag/merge-sort/)


给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

**提示：**

*   `k == lists.length`
*   `0 <= k <= 10^4`
*   `0 <= lists[i].length <= 500`
*   `-10^4 <= lists[i][j] <= 10^4`
*   `lists[i]` 按 **升序** 排列
*   `lists[i].length` 的总和不超过 `10^4`


## Solution

Language: **JavaScript**

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode[]} lists
 * @return {ListNode}
 */
// 递归合并链表+两两合并 reduce merge(l1, l2)
var mergeKLists = function(lists) {
    if (!lists.length) return null
    function merge(l1, l2) {
        if (!l1) return l2
        if (!l2) return l1
        if (l1.val < l2.val) {
            l1.next = merge(l1.next, l2)
            return l1
        } else {
            l2.next = merge(l1, l2.next)
            return l2
        }
    }
    return lists.reduce((pre, cur) => merge(pre, cur))
}
```

![image](https://user-images.githubusercontent.com/59645426/190699226-6f17e3df-d243-4dce-a7ae-3b332964ce4a.png)

![image](https://user-images.githubusercontent.com/59645426/190699538-05ae2620-b6e1-4652-93ac-b1775b24c01a.png)

![image](https://user-images.githubusercontent.com/59645426/190699653-a8b4cf29-d318-4582-aa92-1b1c699e5b6f.png)

![image](https://user-images.githubusercontent.com/59645426/190827369-d8319b43-6357-4955-b40d-95ac0fd0f52a.png)

![image](https://user-images.githubusercontent.com/59645426/190828866-1049c4c3-6a7e-421f-af3b-47c592032c88.png)

![image](https://user-images.githubusercontent.com/59645426/190828891-9a5ce805-f229-4836-9b44-cab4340e975a.png)

![image](https://user-images.githubusercontent.com/59645426/190828906-1eb5e1a7-f066-41f5-8afa-f006b16fcd62.png)

![image](https://user-images.githubusercontent.com/59645426/190828912-e740200f-0149-403c-aed7-f9bc3098fbe4.png)
    
![image](https://user-images.githubusercontent.com/59645426/190828936-6d4352c7-930e-4a69-9502-87c661531a4e.png)

# [25\. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

## Description

Difficulty: **困难**  

Related Topics: [递归](https://leetcode.cn/tag/recursion/), [链表](https://leetcode.cn/tag/linked-list/)


给你链表的头节点 `head` ，每 `k`个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k`的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

**提示：**

*   链表中的节点数目为 `n`
*   `1 <= k <= n <= 5000`
*   `0 <= Node.val <= 1000`

**进阶：**你可以设计一个只用 `O(1)` 额外内存空间的算法解决此问题吗？


## Solution

Language: **JavaScript**

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var reverseKGroup = function(head, k) {
    const hair = new ListNode(0)
    hair.next = head
    let pre = hair
    while (head) {
        let tail = pre
        for (let i = 1; i <= k; i++) {
            tail = tail.next
            if (!tail) {
                return hair.next
            }
        }
        const nex = tail.next;
        [head, tail] = myReverse(head, tail)
        pre.next = head
        tail.next = nex
        pre = tail
        head = tail.next
    }
    return hair.next
};
const myReverse = (head, tail) => {
    let prev = tail.next
    let p = head
    while (prev !== tail) {
        const nex = p.next
        p.next = prev
        prev = p
        p = nex
    }
    return [tail, head]
}

```

# [26\. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

## Description

Difficulty: **简单**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [双指针](https://leetcode.cn/tag/two-pointers/)


给你一个 **升序排列** 的数组 `nums` ，请你** [原地](http://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。

由于在某些语言中不能改变数组的长度，所以必须将结果放在数组nums的第一部分。更规范地说，如果在删除重复项之后有 `k` 个元素，那么 `nums` 的前 `k` 个元素应该保存最终结果。

将最终结果插入 `nums` 的前 `k` 个位置后返回 `k` 。

不要使用额外的空间，你必须在 **[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95) 修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 **通过**。

**示例 1：**

```
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

**示例 2：**

```
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
```

**提示：**

*   1 <= nums.length <= 3 * 10<sup>4</sup>
*   -10<sup>4</sup> <= nums[i] <= 10<sup>4</sup>
*   `nums` 已按 **升序** 排列


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
// 快慢指针 slow 0 fast 0 fast < nums.length
var removeDuplicates = function(nums) {
    let slow = 0
    let fast = 0
    while (fast < nums.length) {
        if (nums[fast] !== nums[slow]) {
            slow++
            nums[slow] = nums[fast]
        }
        fast++
    }
    return slow + 1
};
```

# [28\. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

## Description

Difficulty: **中等**  

Related Topics: [双指针](https://leetcode.cn/tag/two-pointers/), [字符串](https://leetcode.cn/tag/string/), [字符串匹配](https://leetcode.cn/tag/string-matching/)


给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回  `-1`。

**示例 1：**

```
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

**示例 2：**

```
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

**提示：**

*   1 <= haystack.length, needle.length <= 10<sup>4</sup>
*   `haystack` 和 `needle` 仅由小写英文字符组成


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
相关话题：`双指针`、`字符串`
 */
var strStr = function(haystack, needle) {
    if(haystack === needle) return 0
    for(let i = 0; i <= haystack.length - needle.length; i++) {
        for(let j = 0; j < needle.length; j++) {
            if(haystack[i+j] !== needle[j]) break
        }
        if(j === needle.length) return i
    }
    return -1
};
```

# [31\. 下一个排列](https://leetcode.cn/problems/next-permutation/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [双指针](https://leetcode.cn/tag/two-pointers/)


整数数组的一个 **排列**  就是将其所有成员以序列或线性顺序排列。

*   例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

*   例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
*   类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
*   而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须修改，只允许使用额外常数空间。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

**示例 3：**

```
输入：nums = [1,1,5]
输出：[1,5,1]
```

**提示：**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 100`


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
// a[i-1] a[j]
var nextPermutation = function(nums) {
    const len = nums.length
    let i = len - 2
    // 找到第一个当前项比后一项小的位置
    while(i >= 0 && nums[i] >= nums[i+1]) i--
    // i>=0，说明此时不是最大的排序
    if (i >= 0) {
        let j = len - 1
        // 找到最小的比nums[i]大的数对应的j
        while(j > 1 && nums[i] >= nums[j]) j--
        // 交换位置
        [nums[i], nums[j]] = [nums[j], nums[i]]
    }
    // i后面的数升序排序
    let [left, right = [i + 1, len - 1]
    while (left < right) {
        [nums[left], nums[right]] = [nums[right], nums[left]]
        left++
        right--
    }
}
```

# [32\. 最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/)

## Description

Difficulty: **困难**  

Related Topics: [栈](https://leetcode.cn/tag/stack/), [字符串](https://leetcode.cn/tag/string/), [动态规划](https://leetcode.cn/tag/dynamic-programming/)


给你一个只包含 `'('` 和 `')'` 的字符串，找出最长有效（格式正确且连续）括号子串的长度。


**示例 1：**

```
输入：s = "(()"
输出：2
解释：最长有效括号子串是 "()"
```

**示例 2：**

```
输入：s = ")()())"
输出：4
解释：最长有效括号子串是 "()()"
```

**示例 3：**

```
输入：s = ""
输出：0
```

**提示：**

*   0 <= s.length <= 3 * 10<sup>4</sup>
*   `s[i]` 为 `'('` 或 `')'`


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} s
 * @return {number}
 */
// 定义 dp[i] 表示以下标 i 字符结尾的最长有效括号的长度。
/**
)()())

push: 一个参照物；左括号

pop：左括号，一个参照物

保持栈不能为空

[-1]
 */
const longestValidParentheses = (s) => {
    let n = s.length
    let res = 0
    let stack = [-1]
    for (let i = 0; i < n; i++) {
        let str = s[i]
        if (str === '(') {
            stack.push(i)
        } else {
            stack.pop()
            if (stack.length) {
                res = Math.max(res, i - stack[stack.length - 1])
            } else {
                stack.push(i)
            }
        }
    }
    return res
};
```

# [33\. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [二分查找](https://leetcode.cn/tag/binary-search/)


整数数组 `nums` 按升序排列，数组中的值 **互不相同** 。

在传递给函数之前，`nums` 在预先未知的某个下标 `k`（`0 <= k < nums.length`）上进行了 **旋转**，使数组变为 `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`（下标 **从 0 开始** 计数）。例如， `[0,1,2,4,5,6,7]` 在下标 `3` 处经旋转后可能变为 `[4,5,6,7,0,1,2]` 。

给你 **旋转后** 的数组 `nums` 和一个整数 `target` ，如果 `nums` 中存在这个目标值 `target` ，则返回它的下标，否则返回 `-1` 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```
输入：nums = [1], target = 0
输出：-1
```

**提示：**

*   `1 <= nums.length <= 5000`
*   -10<sup>4</sup> <= nums[i] <= 10<sup>4</sup>
*   `nums` 中的每个值都 **独一无二**
*   题目数据保证 `nums` 在预先未知的某个下标上进行了旋转
*   -10<sup>4</sup> <= target <= 10<sup>4</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 * 二分搜索
 * 关键字：排序，搜索
 */
var search = function(nums, target) {
    let [left, right] = [0, nums.length - 1]

    while (left <= right) {
        const mid = (left + right) >>> 1
        if (nums[mid] === target) return mid

        if (nums[mid] > nums[right]) {
            if (nums[left] <= target && nums[mid] > target) right = mid - 1
            else left = mid + 1
        } else {
            if (nums[mid] < target && nums[right] >= target) left = mid + 1
            else right = right - 1
        }
    }
    return -1
}
```

# [34\. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [二分查找](https://leetcode.cn/tag/binary-search/)


给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

**提示：**

*   0 <= nums.length <= 10<sup>5</sup>
*   -10<sup>9</sup> <= nums[i] <= 10<sup>9</sup>
*   `nums` 是一个非递减数组
*   -10<sup>9</sup> <= target <= 10<sup>9</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var searchRange = function(nums, target) {
    let [start, end] = [0, nums.length - 1]
    while(start <= end) {
        const mid = (start + end) >>> 1
        if (nums[mid] === target) {
            [start, end] = [mid, mid]
            break
        }
        nums[mid] > target ? (end = mid - 1) : (start = mid + 1)
    }
    while (nums[start - 1] === target) start--
    while (nums[end + 1] === target) end++
    return start > end ? [-1, -1] : [start, end]
}
```

# [39\. 组合总和](https://leetcode.cn/problems/combination-sum/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [回溯](https://leetcode.cn/tag/backtracking/)


给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 _所有 _**不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []
```

**提示：**

*   `1 <= candidates.length <= 30`
*   `1 <= candidates[i] <= 200`
*   `candidate` 中的每个元素都 **互不相同**
*   `1 <= target <= 500`


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} candidates
 * @param {number} target
 * @return {number[][]}
 */
// 搜索回溯
var combinationSum = function(candidates, target) {
    const ans = []
    const dfs = (target, item, idx) => {
        if (idx === candidates.length) return
        if (target === 0) {
            ans.push(item)
            return
        }
        dfs(target, item, idex + 1)
        if (target - candidates[idx] >= 0) {
            dfs(target - candidates[idx], [...item, candidates[idx]], idx)
        }
    }
    dfs(target, [], 0)
    return ans
}
```

# [42\. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

## Description

Difficulty: **困难**  

Related Topics: [栈](https://leetcode.cn/tag/stack/), [数组](https://leetcode.cn/tag/array/), [双指针](https://leetcode.cn/tag/two-pointers/), [动态规划](https://leetcode.cn/tag/dynamic-programming/), [单调栈](https://leetcode.cn/tag/monotonic-stack/)


给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例 1：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例 2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

**提示：**

*   `n == height.length`
*   1 <= n <= 2 * 10<sup>4</sup>
*   0 <= height[i] <= 10<sup>5</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
// 动态规划
var trap = function(height) {
    const n = height.length
    const left = new Array(n).fill(0)
    const right = new Array(n).fill(0)
    let ans = 0
    for (let i = 1; i < n; i++) {
        left[i] = Math.max(left[i-1], height[i-1])
    }
    for (let i = n - 2; i >= 0; i--) {
        right[i] = Math.max(right[i+1], height[i+1])
        let short = Math.min(left[i], right[i]
        if (short > height[i]) {
            ans += short - height[i]
        }
     }
     return ans
}

```

# [46\. 全排列](https://leetcode.cn/problems/permutations/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [回溯](https://leetcode.cn/tag/backtracking/)


给定一个不含重复数字的数组 `nums` ，返回其 _所有可能的全排列_ 。你可以 **按任意顺序** 返回答案。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

**提示：**

*   `1 <= nums.length <= 6`
*   `-10 <= nums[i] <= 10`
*   `nums` 中的所有整数 **互不相同**


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 * 广度优先遍历
 * 深度优先遍历
 */
var permute = function(nums) {
    const ans = []
    const dfs = (item = []) => {
        if (item.length === nums.length) return ans.push([...item])
        
        nums.forEach(num => {
            if (!item.includes(num)) {
                dfs([...item, num]
            }
        })
    }
    dfs()
    return ans
}
```

# [48\. 旋转图像](https://leetcode.cn/problems/rotate-image/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [数学](https://leetcode.cn/tag/math/), [矩阵](https://leetcode.cn/tag/matrix/)


给定一个 _n _× _n_ 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

```
输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

**提示：**

*   `n == matrix.length == matrix[i].length`
*   `1 <= n <= 20`
*   `-1000 <= matrix[i][j] <= 1000`


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[][]} matrix
 * @return {void} Do not return anything, modify matrix in-place instead.
 */
// 使用辅助数组
// 对于矩阵中第 i 行的第 j 个元素，在旋转后，它出现在倒数第 i 列的第 j 个位置。
// matrix[row][col] matrix_new[col][n-row-1]
var rotate = function(matrix) {
    const n = matrix.length
    const matrix_new = new Array(n).fill(0).map(() => new Array(n).fill(0))
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            matrix_new[j][n-1-i] = matrix[i][j]
        }
    }
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            matrix[i][j] = matrix_new[i][j]
        }
    }
}
```

# [49\. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [哈希表](https://leetcode.cn/tag/hash-table/), [字符串](https://leetcode.cn/tag/string/), [排序](https://leetcode.cn/tag/sorting/)


给你一个字符串数组，请你将 **字母异位词** 组合在一起。可以按任意顺序返回结果列表。

**字母异位词** 是由重新排列源单词的字母得到的一个新单词，所有源单词中的字母通常恰好只用一次。

**示例 1:**

```
输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**示例 2:**

```
输入: strs = [""]
输出: [[""]]
```

**示例 3:**

```
输入: strs = ["a"]
输出: [["a"]]
```

**提示：**

*   1 <= strs.length <= 10<sup>4</sup>
*   `0 <= strs[i].length <= 100`
*   `strs[i]` 仅包含小写字母


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string[]} strs
 * @return {string[][]}
 */
var groupAnagrams = function(strs) {
    const map = new Map()
    for (let str of strs) {
        let array = Array.from(str)
        array.sort()
        
        let key = array.toString()
        let list = map.get(key) ? map.get(key) : []
        list.push(str)
        
        map.set(key, list)
    }
    return Array.from(map.values())
};
```

# [53\. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [分治](https://leetcode.cn/tag/divide-and-conquer/), [动态规划](https://leetcode.cn/tag/dynamic-programming/)


给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

**提示：**

*   1 <= nums.length <= 10<sup>5</sup>
*   -10<sup>4</sup> <= nums[i] <= 10<sup>4</sup>

**进阶：**如果你已经实现复杂度为 `O(n)` 的解法，尝试使用更为精妙的 **分治法** 求解。


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
// 动态规划
var maxSubArray = function(nums) {
    let pre = 0
    let maxAns = nums[0]
    nums.forEach((x) => {
        pre = Math.max(pre + x, x)
        maxAns = Math.max(maxAns, pre)
    })
    return maxAns
}
```

# [55\. 跳跃游戏](https://leetcode.cn/problems/jump-game/)

## Description

Difficulty: **中等**  

Related Topics: [贪心](https://leetcode.cn/tag/greedy/), [数组](https://leetcode.cn/tag/array/), [动态规划](https://leetcode.cn/tag/dynamic-programming/)


给定一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

**示例 1：**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

**示例 2：**

```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

**提示：**

*   1 <= nums.length <= 3 * 10<sup>4</sup>
*   0 <= nums[i] <= 10<sup>5</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @return {boolean}
 */
// 贪心
// var canJump = function(nums) {
//     // 如果只有一个，返回true
//     if (nums.length === 1) return true
//     // 记录所能到达的最大位置
//     var res = nums[0]
//     for (let i = 1; i < nums.length; i++) {
//         // 如果最大位置不能到达当前位置，则跳出
//         if (res < i) break
//         // 如果最大位置超过最后一个位置，则返回true
//         if (res >= nums.length - 1) return true
//         // 计算能到达的最大位置
//         res = res > nums[i] + i ? res : nums[i] + i
//     }
//     return false
// }

// 贪心
var canJump = function(nums) {
    let n = nums.length - 1
    let maxLen = 0
    for (let i = 0; i <= maxLen; i++) {
        maxLen = Math.max(maxLen, nums[i] + i)
        if (maxLen >= n) return true
    }
    return false
}

// 动态规划
// var canJump = function(nums) {
//     let end = nums.length - 1

//     for (let i = nums.length - 2; i >= 0; i--) {
//         if (end - i <= nums[i]) {
//             end = i
//         }
//     }

//     return end === 0
// }

```

# [56\. 合并区间](https://leetcode.cn/problems/merge-intervals/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [排序](https://leetcode.cn/tag/sorting/)


以数组 `intervals` 表示若干个区间的集合，其中单个区间为 intervals[i] = [start<sub>i</sub>, end<sub>i</sub>] 。请你合并所有重叠的区间，并返回 _一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间_ 。

**示例 1：**

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

**示例 2：**

```
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

**提示：**

*   1 <= intervals.length <= 10<sup>4</sup>
*   `intervals[i].length == 2`
*   0 <= start<sub>i</sub> <= end<sub>i</sub> <= 10<sup>4</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */

var merge = function (intervals) {
    let res = []
    intervals.sort((a, b) => a[0] - b[0])
    let prev = intervals[0]
    for (let i = 1; i < intervals.length; i++) {
        let cur = intervals[i]
        if (prev[1] >= cur[0]) {
            prev[1] = Math.max(prev[1], cur[1])
        } else {
            res.push(prev)
            prev = cur
        }
    }
    res.push(prev)
    return res
}

```

# [62\. 不同路径](https://leetcode.cn/problems/unique-paths/)

## Description

Difficulty: **中等**  

Related Topics: [数学](https://leetcode.cn/tag/math/), [动态规划](https://leetcode.cn/tag/dynamic-programming/), [组合数学](https://leetcode.cn/tag/combinatorics/)


一个机器人位于一个 `m x n`网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

**示例 1：**

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
输入：m = 3, n = 7
输出：28
```

**示例 2：**

```
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1\. 向右 -> 向下 -> 向下
2\. 向下 -> 向下 -> 向右
3\. 向下 -> 向右 -> 向下
```

**示例 3：**

```
输入：m = 7, n = 3
输出：28
```

**示例 4：**

```
输入：m = 3, n = 3
输出：6
```

**提示：**

*   `1 <= m, n <= 100`
*   题目数据保证答案小于等于 2 * 10<sup>9</sup>


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number} m
 * @param {number} n
 * @return {number}
 * 动态规划 每一格的路径由其上一格和左一格决定。
 * 动态方程：dp[i][j] = dp[i-1][j] + dp[i][j-1]
 * 注意：对于第一行 dp[0][j]，或者第一列 dp[i][0]，由于都是在边界，所以只能为1
 * 建立m*n的矩阵，注意第0行和第0列元素均为1
 */
var uniquePaths = function(m, n) {
    const dp = new Array(m).fill(0).map(() => new Array(n).fill(0))
    
    for (let i = 0; i < m; i++) {
        dp[i][0] = 1
    }
    for (let j = 0; j < n; j++) {
        dp[0][j] = 1
    }
    
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
        }
    }
    return dp[m-1][n-1]
}

```

# [64\. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [动态规划](https://leetcode.cn/tag/dynamic-programming/), [矩阵](https://leetcode.cn/tag/matrix/)


给定一个包含非负整数的 `_m_ x _n_` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

**示例 2：**

```
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

**提示：**

*   `m == grid.length`
*   `n == grid[i].length`
*   `1 <= m, n <= 200`
*   `0 <= grid[i][j] <= 100`


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[][]} grid
 * @return {number}
 */
var minPathSum = function(grid) {
    if (grid === null || grid.length === 0 || grid[0].length === 0) {
        return 0
    }
    let row = grid.length, col = grid[0].length
    const dp = new Array(row).fill(0).map(() => new Array(col).fill(0))
    dp[0][0] = grid[0][0]
    for (let i = 1; i < row; i++) {
        dp[i][0] = dp[i-1][0] + grid[i][0]
    }
    for (let j = 1; j < col; j++) {
        dp[0][j] = dp[0][j-1] + grid[0][j]
    }
    for (let i = 1; i < row; i++) {
        for (let j = 1; j < col; j++) {
            dp[i][j] = Math.min(dp[i][j-1], dp[i-1][j]) + grid[i][j]
        }
    }
    return dp[row-1][col-1]
}

```

# [70\. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

## Description

Difficulty: **简单**  

Related Topics: [记忆化搜索](https://leetcode.cn/tag/memoization/), [数学](https://leetcode.cn/tag/math/), [动态规划](https://leetcode.cn/tag/dynamic-programming/)


假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1\. 1 阶 + 1 阶
2\. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1\. 1 阶 + 1 阶 + 1 阶
2\. 1 阶 + 2 阶
3\. 2 阶 + 1 阶
```

**提示：**

*   `1 <= n <= 45`


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number} n
 * @return {number}
 * 题目分析:
 * 第1级台阶：1种方法（爬1级）
 * 第2级台阶：2种方法（爬1级或爬2级）
 * 第n级台阶：dp[i] = dp[i-1] + dp[i-2]
 */
// 动态规划
// 滚动数组思想
// var climbStairs = function(n) {
//     if (n === 1) return 1
//     if (n === 2) return 2
//     let [pre, cur, sum] = [1, 1, 2]
//     for (let i = 3; i <= n; i++) {
//         [pre, cur] = [cur, sum]
//         sum = pre + cur
//     }
//     return sum
// }

var climbStairs = function(n) {
    const dp = []
    dp[0] = 1
    dp[1] = 1
    for (let i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2]
    }
    return dp[n]
}

```

# [72\. 编辑距离](https://leetcode.cn/problems/edit-distance/)

## Description

Difficulty: **困难**  

Related Topics: [字符串](https://leetcode.cn/tag/string/), [动态规划](https://leetcode.cn/tag/dynamic-programming/)


给你两个单词 `word1` 和 `word2`， _请返回将 `word1` 转换成 `word2` 所使用的最少操作数_  。

你可以对一个单词进行如下三种操作：

*   插入一个字符
*   删除一个字符
*   替换一个字符

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

**提示：**

*   `0 <= word1.length, word2.length <= 500`
*   `word1` 和 `word2` 由小写英文字母组成


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} word1
 * @param {string} word2
 * @return {number}
 */
// dp[i][j]是word1,word2的最小编辑距离
var minDistance = function(word1, word2) {
    let m = word1.length
    let n = word2.length
    const dp = new Array(m+1).fill(0).map(() => new Array(n+1).fill(0))
    // 初始化
    for (let i = 1; i <= m; i++) {
        dp[i][0] = i
    }
    for (let j = 1; j <= n; j++) {
        dp[0][j] = j
    }
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i-1] === word2[j-1]) {
                dp[i][j] = dp[i-1][j-1]
            } else {
                dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
            }
         }
     }
     return dp[m][n]
}

```

# [75\. 颜色分类](https://leetcode.cn/problems/sort-colors/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [双指针](https://leetcode.cn/tag/two-pointers/), [排序](https://leetcode.cn/tag/sorting/)


给定一个包含红色、白色和蓝色、共 `n`个元素的数组 `nums` ，对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 `0`、 `1` 和 `2` 分别表示红色、白色和蓝色。

必须在不使用库的sort函数的情况下解决这个问题。

**示例 1：**

```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

**示例 2：**

```
输入：nums = [2,0,1]
输出：[0,1,2]
```

**提示：**

*   `n == nums.length`
*   `1 <= n <= 300`
*   `nums[i]` 为 `0`、`1` 或 `2`

**进阶：**

*   你可以不使用代码库中的排序函数来解决这道题吗？
*   你能想出一个仅使用常数空间的一趟扫描算法吗？


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @return {void}
 * 
 */
// 遇到0，把0放在数组最前方，此时i位置没变，不用处理
// 遇到2，把2放在数组最后放，此时i向后偏移了1，需要进行i--，然后数组末尾的2已经被统计过了，所以len可以对应再减1，避免重复的问题

var sortColors = function(nums) {
    let len = nums.length
    for (let i = 0; i < len; i++) {
        if (nums[i] === 0) {
            nums.splice(i, 1)
            nums.unshift(0)
        } else if (nums[i] === 2) {
            nums.splice(i, 1)
            nums.push(2)
            len--
            i--
        }
    }
}

```

# [76\. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)

## Description

Difficulty: **困难**  

Related Topics: [哈希表](https://leetcode.cn/tag/hash-table/), [字符串](https://leetcode.cn/tag/string/), [滑动窗口](https://leetcode.cn/tag/sliding-window/)


给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

**注意：**

*   对于 `t` 中重复字符，我们寻找的子字符串中该字符数量必须不少于 `t` 中该字符数量。
*   如果 `s` 中存在这样的子串，我们保证它是唯一的答案。

**示例 1：**

```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
```

**示例 2：**

```
输入：s = "a", t = "a"
输出："a"
```

**示例 3:**

```
输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。
```

**提示：**

*   1 <= s.length, t.length <= 10<sup>5</sup>
*   `s` 和 `t` 由英文字母组成

**进阶：**你能设计一个在 `o(n)` 时间内解决此问题的算法吗？

## Solution

Language: **JavaScript**

```javascript
/**
 * @param {string} s
 * @param {string} t
 * @return {string}
 */
// 将需要匹配的字符放入字典表，存储结构为需要匹配的字符，以及字符出现的次数：[字符，次数]
// 利用双指针维护一个滑动窗口，不断移动右指针
// 判断右指针的字符是否与字典表中的匹配
// 是：将字典表中的次数-1，直到为0（创建一个needType）记录需要匹配的字符数量，初始长度为Map的size，当对应的字符次数为0时，就减1
// 否：继续移动右指针
// 当needType的值为0时，就证明在当前窗口所有字符都匹配成功了
// 1.移动左指针，缩小滑动窗口的大小
// 2.移动过程中判断左指针指向的值是否为字典中值，如果是就证明匹配的值少了一个，这个需要更新Map中的次数，以及needType的数量
// 3.记录每次窗口中的字符，找到最小的返回
var minWindow = function(s, t) {
    // 创建左指针
    let l = 0
    // 创建右指针
    let r = 0
    // 最后需要返回的最小长度子串
    let res = ''
    // 创建字典表
    const m = new Map()
    // 遍历需要匹配的字符
    for (let i = 0; i < t.length; i++) {
        const c = t[i]
        // 放入字典表
        m.set(c, m.has(c) ? m.get(c) + 1 : 1)
    }
    // 创建记录需要匹配的字符种类
    let needType = m.size
    // 遍历字符串
    while (r < s.length) {
        // 获取当前字符
        const c = s[r]
        // 如果是需要匹配的字符
        if (m.has(c)) {
            // 更新字典表中的次数-1
            m.set(c, m.get(c) - 1)
            // 如果次数为0， 证明这个字符种类在当前窗口已经集起了, needType-1
            if (m.get(c) === 0) needType -= 1
        }
        // 当needType为0，证明所有需要匹配的字符串都已经在当前滑动窗口中
        while (needType === 0) {
            const c2 = s[l]
            // 记录当前滑动窗口的字符
            let numRes = s.slice(l, r + 1)
            // 当新的窗口中字符长度小于上次的字符长度时，更新结果
            // !res 是在结构值为空的时候需要更新一下第一次匹配的值
            if (!res || newRes.length < res.length) res = newRes
            // 如果左指针移动过程中出现，字典中的值证明需要匹配的字符已经脱离了当前窗口
            if (m.has(c2)) {
                // 更新表中需要出现的次数
                m.set(c2, m.get(c2) + 1)
                // 更新needType
                if (m.get(c2) === 1) needType += 1
            }
            // 移动左指针
            l++
        }
        // 移动右指针
        r++
    }
    // 返回结果值
    return res
}
```

# [78\. 子集](https://leetcode.cn/problems/subsets/)

## Description

Difficulty: **中等**  

Related Topics: [位运算](https://leetcode.cn/tag/bit-manipulation/), [数组](https://leetcode.cn/tag/array/), [回溯](https://leetcode.cn/tag/backtracking/)


给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

**提示：**

*   `1 <= nums.length <= 10`
*   `-10 <= nums[i] <= 10`
*   `nums` 中的所有元素 **互不相同**


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
// 递归法实现子集枚举
// var subsets = function (nums) {
//     let result = []
//     let path = []
//     function backtracking(startIndex) {
//         result.push(path.slice())
//         for (let i = startIndex; i < nums.length; i++) {
//             path.push(nums[i])
//             backtracking(i + 1)
//             path.pop()
//         }
//     }
//     backtracking(0)
//     return result
// }

// DFS回溯思路
var subsets = function (nums) {
    const res = []
    const dfs = (index, list) => {
        res.push(list.slice())
        for (let i = index; i < nums.length; i++) {
            list.push(num[i])
            dfs(i+1, list)
            list.pop()
        }
    }
    dfs(0, [])
    return res
}

```

# [79\. 单词搜索](https://leetcode.cn/problems/word-search/)

## Description

Difficulty: **中等**  

Related Topics: [数组](https://leetcode.cn/tag/array/), [回溯](https://leetcode.cn/tag/backtracking/), [矩阵](https://leetcode.cn/tag/matrix/)


给定一个 `m x n` 二维字符网格 `board` 和一个字符串单词 `word` 。如果 `word` 存在于网格中，返回 `true` ；否则，返回 `false` 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
输出：true
```

**示例 3：**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
输出：false
```

**提示：**

*   `m == board.length`
*   `n = board[i].length`
*   `1 <= m, n <= 6`
*   `1 <= word.length <= 15`
*   `board` 和 `word` 仅由大小写英文字母组成

**进阶：**你可以使用搜索剪枝的技术来优化解决方案，使其在 `board` 更大的情况下可以更快解决问题？


## Solution

Language: **JavaScript**

```javascript
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
// 记录该元素已使用
// 上下左右不能超边界
// 如果没找到最终值的时候恢复上一个节点的值
var exist = function(board, word) {
    // 越界处理
    board[-1] = [] // 这里处理比较巧妙，利用了js的特性
    board.push([])
    // 寻找首个字母
    for (let y = 0; y < board.length; y++) {
        for (let x = 0; x < board[0].length; x++) {
            if (word[0] === board[y][x] && dfs(word, board
        }
    }
    return false
}

const dfs = function(word, board, y, x, i) {
    if (i + 1 === word.length) return true
    var tmp = board[y][x]
    //标记该元素已使用
    board[y][x] = false
    if (board[y][x+1] === word[i+1] && dfs(word, board, y, x+1, i+1)) return true
    if (board[y][x-1] === word[i+1] && dfs(word, board, y, x-1, i+1)) return true
    if (board[y+1][x] === word[i+1] && dfs(word, board, y+1, x, i+1)) return true
    if (board[y-1][x] === word[i+1] && dfs(word, board, y-1, x, i+1)) return true
    // 回溯
    board[y][x] = tmp
}
```

























