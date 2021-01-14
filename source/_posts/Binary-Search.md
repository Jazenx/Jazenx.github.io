---
title: 二分查找 Binary Search
date: 2020-09-11 20:38:18
tags: Algorithm
---

## 什么是二分查找

二分查找也常被称为二分法或者折半查找，每次查找时通过将待查找区间分成两部分并只取一部分继续查找，将查找的复杂度大大减少。对于一个长度为 O(n) 的数组，二分查找的时间复 杂度为 O(log n)。

二分查找的前提是：

1. 目标函数单调性（单调递增或者递减）
2. 存在上下边界
3. 能够通过索引访问

二分查找时区间的左右端取开区间还是闭区间在绝大多数时候都可以。尝试熟练使用 一种写法，比如左闭右开或者左闭右闭。我们还需要思考如果最后区间只剩下一个数或者两个数，自己的写法是否会陷入死循环，如果某种写法无法跳出死循环，则考虑尝试另一种写法。

二分查找也可以看作双指针的一种特殊情况，但我们一般会将二者区分。双指针类型的题， 指针通常是一步一步移动的，而在二分查找里，指针每次移动半个区间长度。

下面是 js 中二分查找的一个通用模板：

```js
// 二分查找
function binarySearch(array, target) {
  let left = 0
  let right = array.length - 1
  while (left <= right) {
	let mid = (left + right) >> 1
	if (array[mid] === target) {
	  /* find the target */
	  return
	} else if (array[mid] < target) {
	  left = mid + 1
	} else {
	  right = mid - 1
	}
  }
}

```

刷到二分查找的题目的时候，一定要细致，判断好所有的边界条件!

## Let’s leetcode！

### 求开方

#### [x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

解题思路：

- 题目可以理解成求 f (z) = z^2 − x= 0 的解
- 通过二分法去逼近符合条件的整数

```js
const mySqrt = function (x) {
  if (x === 0 || x === 1) return x
  let left = 1
  let right = x
  let res = null
  while (left <= right) {
	const mid = parseInt((left + right) / 2);
	if (mid === x / mid) {
	  return mid
	} else if (mid > x / mid) {
	  right = mid - 1
	} else {
	  left = mid + 1
	}
  }
  return right
};
```

### 查找区间

#### [在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

解题思路：

- 数组已经单调递增进行排序，所以可以使用二分法来加速查找。
- 可以写一个二分查找的方法。找到 target 值，然后再不断逼近。

```js
var searchRange = function (nums, target) {
  let index = binarySearch(nums, target)
  console.log(index)
  if (index === -1) return [-1, -1]
  if(nums.length === 1) return [index, index]
  let left = index
  let right = index + 1
  while (nums[left] === target && left >= 0) left -= 1
  while (nums[right] === target && right < nums.length) right += 1
  return [left + 1, right - 1]
};

var binarySearch = (nums, target, l = 0, r = nums.length - 1) => {
  if (l > r) return -1
  const mid = l + r >> 1
  console.log(mid)
  if (nums[mid] === target) { 
	return mid 
  }
  else if (nums[mid] > target) {
   return binarySearch(nums, target, l, mid - 1)
  } else {
   return binarySearch(nums, target, mid + 1, r)
  }
} 
```

### 旋转数组查找数字

#### [搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,0,1,2,2,5,6] 可能变为 [2,5,6,0,0,1,2] )。

编写一个函数来判断给定的目标值是否存在于数组中。若存在返回 true，否则返回 false。

示例 1:

> 输入: nums = [2,5,6,0,0,1,2], target = 0

> 输出: true

示例 2:

> 输入: nums = [2,5,6,0,0,1,2], target = 3

> 输出: false

解题思路：

1. 暴力法：还原升序后查找
2. 二分法：存在不是严格意义上的单调

```js
var search = function (nums, target) {
  if (nums.length < 0) return false
  let l = 0
  let r = nums.length - 1
  while (l <= r) {
	const mid = l + r >> 1
	if (nums[mid] === target) {
	  return true
	}
	// mid 左边有序
	if (nums[mid] > nums[l]) {
	  if (target >= nums[l] && target < nums[mid]) {
		r = mid
	  } else {
		l = mid + 1
	  }
	} else if (nums[mid] < nums[l]) {
	  if (target > nums[mid] && target <= nums[r]) {
		l = mid + 1
	  }
	  else {
		r = mid
	  }
	} else if (nums[mid] === nums[l]) {
	  l++
	}
  }
  return false
}
```

## 参考资料

[changgyhub/leetcode_101](https://github.com/changgyhub/leetcode_101)
