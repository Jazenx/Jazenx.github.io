---
title: 贪心算法 Greedy
date: 2020-6-19 10:54:58
tags: Algorithm
---
## 什么是贪心算法？

贪心算法采用贪心的策略，保证每次操作都是局部最优的，从而使结果是全局最优。

意思是我每一步都是最贪的，那么我全局也是最贪的。

那么什么时候使用贪心算法？

问题能够拆分成子问题来解决，子问题的最优解能递推到最终问题的最优解。即存在最优子结构。最优子结构的意思是局部最优解能决定全局最优解。

贪心算法与[动态规划](https://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92 "动态规划")的不同在于它对每个子问题的解决方案都做出选择，不能回退。动态规划则会保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能。

贪心算法有它的局限性，局部都是最优全局有可能不是最优解。但是一旦一个问题可以用贪心算法来解决，那么贪心算法一般是解决此问题的最优解。

贪心算法步骤：

1. 创建数学模型来描述问题。
2. 把求解的问题分成若干个**子问题**。
3. 对每一子问题求解，得到子问题的局部最优解。
4. 把子问题的解局部最优解合成原来解问题的一个解。

实现该算法的过程：
从问题的某一初始解出发；while 能朝给定总目标前进一步 do，求出可行解的一个解元素；
最后，由所有解元素组合成问题的一个可行解。

### Let‘s leetcode！

### 分配问题

#### [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 i，都有一个胃口值 g[i]，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j，都有一个尺寸 s[j] 。如果 s[j] >= g[i]，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

个人思路：

- 子问题：每个小孩如何吃饼。
- 贪心：每个孩子能吃到满足他胃口的且数量最少的饼。
- 结果：尽可能多的孩子吃到满足自己需求的饼。

解题思路：

- 将孩子和饼干数组从小大排序，用来实现最小孩子吃到最小满足需求的饼干的需求。
- 然后设置 child 和 cookie 指针去遍历上面两个数组。
- 进入循环：
   - 判断当前 cookie 是否能满足孩子需要，即大于等于当前孩子胃口。
   - 如果满足，则child 加一，继续去判断下一个孩子，并且 cookie 加一。
   - 如果不满足。则直接 cookie 加一，判断下一个 cookie 是否满足。
- 最后返回child 的值就是能吃饱的孩子个数
  
```js
const findContentChildren = function(g, s) {
  const childen = g.sort((a, b) => a - b)
  const cookies = s.sort((a, b) => a - b)
  let child = 0
  let cookie = 0
  while(child < childen.length && cookie < cookies.length) {
	if (childen[child] <= cookies[cookie]) child++
	  cookie ++
  }
  return child
}; 
```

#### [135. 分发糖果](https://leetcode-cn.com/problems/candy/)

老师想给孩子们分发糖果，有 *N* 个孩子站成了一条直线，老师会根据每个孩子的表现，预先给他们评分。

你需要按照以下要求，帮助老师给 这些孩子分发糖果：

- 每个孩子至少分配到 1 个糖果。
- 评分更高的孩子必须比他两侧的邻位孩子获得更多的糖果。

那么这样下来，老师至少需要准备多少颗糖果呢？

个人思路:

- 首先每个小孩先发一颗糖
- 然后从左往右，比较分数。要是右边孩子比左边孩子分数高，则右边孩子的糖为左边孩子糖的个数加一。
- 接着从右往左，比较分组，要是左边孩子比右边孩子分数高，且左边孩子的糖比右边的少，则由左孩子的糖为右边孩子糖的个数加一。
- 贪心：每次只考虑孩子与相邻孩子比较分数。
  
```js
const candy = function (ratings) {
  const candy = new Array(ratings.length).fill(1);
  for (let child = 1; child < ratings.length; child ++) {
	if (ratings[child] > ratings[child - 1])
	  candy[child] = candy[child - 1] + 1;
  }
  for (let child = ratings.length - 2; child >= 0; child --) {
	if (ratings[child] > ratings[child + 1])
	  candy[child] = Math.max(candy[child + 1] + 1, candy[child]);
  }
  return candy.reduce((a, b) => a + b);
};
```


### 区间问题

#### [435. 无重叠问题](https://leetcode-cn.com/problems/non-overlapping-intervals/)

给定一个区间的集合，找到需要移除区间的最小数量，使剩余区间互不重叠。

个人思路：

- 数组区间越小，即区间结尾越小，留给其他区间的空间越大，越不容易重叠。
- 贪心：优先保留区间结尾小，但是不相交的数组。

解题思路：

- 数组按照区间结尾的大小进行升序。
- 然后判断两数组首尾是否相交。

注意点：

- JavaScript 中有个测例需要判空，可以用数组长度为 0 去判断。

```js
const eraseOverlapIntervals = function (intervals) {
  if(intervals.length === 0 || intervals == null) return 0
  intervals.sort((a, b) => a[1] - b[1])
  let total = 0
  let prev = intervals[0][1]
  for (let i = 1; i < intervals.length; i += 1) {
	  intervals[i][0] < prev ? total += 1 : prev = intervals[i][1]
  }
  return total
}; 
```

## 参考资料

[贪心算法 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95)
[changgyhub/leetcode_101](https://github.com/changgyhub/leetcode_101)


