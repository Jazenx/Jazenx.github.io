---
title: 双指针 Two Pointers
date: 2020-01-05 09:56:13
tags: Algorithm
---
## 什么是双指针

使用两个指针指向不同的元素，然后遍历数组，从而完成任务。这就是双指针。

如果两个指针指向同一数组，遍历方向相同且不会相交，这就是滑动窗口，以两指针为收尾的区间就是当前的窗口。我们可以用其来进行区间搜索。

如果两个指针指向同一数组，但是遍历方向相反，则可以用来进行搜索，一般来说待搜索的数组是排好序的。

## Let‘s leetcode！

### Two Sum

#### [两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

给定一个已按照升序排列的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2。

个人思路：

- 已知升序排列的有序数组。
- left：一个指针指向最小的元素
- right： 一个指正指向最大的元素
- left + right：
   - 等于目标数，直接返回 [left, right]
   - 小于目标数，left 向右移动一格下标
   - 大于目标数，right 向左移动一格下标
- 如果 left 等于 right 下标，证明没有符合条件的，返回 null
  
```js
var twoSum = function (numbers, target) {
  let left = 0
  let right = numbers.length - 1
  while (left !== right) {
	if (numbers[left] + numbers[right] === target) break
	if (numbers[left] + numbers[right] < target) left += 1
	if (numbers[left] + numbers[right] > target) right -= 1
  }
  return [left + 1, right + 1]
};
```

## 归并两个有序数组

#### [合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

给你两个有序整数数组 *nums1* 和 *nums2*，请你将 *nums2* 合并到 *nums1* 中*，*使 *nums1* 成为一个有序数组。

个人思路：

- 两个指针 p1，p2分别指向两个有序数组的末尾。
- 设置一个p指针来追踪元素的位置。 p = m + n - 1
- 当 *nums2[*p2] > *nums1[*p1] 时，p 指针下标存 p2 下标的值，并且 p2 下标移动一位，p 指针也指向下一位，p2—， p—
- 当 *nums2[*p2] <= *nums1[*p1]  时，p 指针下标存 p1 下标的值，并且 p1 下标移动一位，p 指针也指向下一位，p1—， p—
- 当 p 指针指向 0 下标的时候结束。返回数组
  
```js
var merge = function (nums1, m, nums2, n) {
  let p1 = m - 1
  let p2 = n - 1
  let p = m + n - 1
  while (p2 >= 0) {
	nums1[p--] = (nums1[p1] > nums2[p2]) ? nums1[p1--] : nums2[p2--]
  }
  return nums1
};
```

### 快慢指针

#### [环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。**注意，`pos` 仅仅是用于标识环的情况，并不会作为参数传递到函数中。**

个人思路：

- 对于这种链表找环路的问题，有一个通用的解法就是快慢指针。
- 快慢指针同时从链表开头出发，快指针一次走 2 步，慢指针一次走 1 步。
- 如果快指针可以走到尽头，则无环路。
- 如果快指针与慢指针相遇，则代表有环。
- 当第一次快慢指针相遇的时候，选一个新指针从链表开头出发，每次走一步，慢指针依然前进一步。新指针和慢指针相交的位置就是环路的入口。
  
```js
var detectCycle = function(head) {
  if(head === null) return null
  let slow = head
  let fast = head
  while(fast !== null) {
	slow = slow.next
	if(fast.next !== null) {
	  fast = fast.next
	} else {
	  return null
	}
	if(slow === fast) {
	  let cyc = head
	  while(cyc !== slow) {
		cyc = cyc.next
		slow = slow.next
	  }
	  return cyc
	}
  }
  return null
};
```

### 滑动窗口

#### [最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

个人思路：

- 使用左右指针，指向字符串初始位置
- 用一个 map 来存储字符串 t 中元素
- 滑动窗口：
   - 从第一个元素开始，如果判断是否有 map 中的元素。有的话就减1。然后右指针依次加一
   - 当map 的大小为 0 的时候，证明当前字符串中有符合条件的子串。然后将左指针加 1，判断这个子串是否符合子串。然后一步步逼近最小子串。
  
```js
var minWindow = function (s, t) {
  let l = 0
  let r = 0
  let map = new Map()
  let res = ''
  for (let i of t) {
	map.set(i, map.has(i) ? map.get(i) + 1 : 1)
  }
  let size = map.size
  while (r < s.length) {
	const word = s[r]
	if (map.has(word)) {
	  map.set(word, map.get(word) - 1)
	  if (map.get(word) === 0) size -= 1
	}
	while (size === 0) {
	  const newRes = s.substring(l, r + 1)
	  if (!res || res.length > newRes.length) res = newRes
	  const word2 = s[l]
	  if (map.has(word2)) {
		map.set(word2, map.get(word2) + 1)
		if (map.get(word2) === 1) size += 1
	  }
	  l += 1
	}
	r += 1
  }
  return res
};
```

## 参考资料

[changgyhub/leetcode_101](https://github.com/changgyhub/leetcode_101)
[力扣](https://leetcode-cn.com/problems/linked-list-cycle-ii/solution/linked-list-cycle-ii-kuai-man-zhi-zhen-shuang-zhi-/)





