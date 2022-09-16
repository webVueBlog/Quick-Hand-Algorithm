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
















