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



























