---
title: 排序算法
date: 2021-01-14 17:29:50
tags: Algorithm
---
## 排序算法种类 

![Image](https://res.craft.do/user/preview/6fb64177-8f69-2661-4f82-68e681c36ecc/doc/993142F9-3B85-432E-92C9-9B20C518E1FF/A481C3DE-ED0D-477C-A6B3-2F015B18209A_1)


排序算法主要分两大类：

1. 比较类排序算法
   - 比较类排序通过比较来决定元素间的相对次序，由于期时间复杂度不能突破 O(nlogn) ，因此也称为非线性时间比较类排序。
2. 非比较类排序算法
   - 不通过比较来决定元素间的相对次序，它可以突破基于比较排序的时间下界，以线性时间运行，因此也称为线性时间非比较类排序。这种一般是对于整型的元素来做，同时它一般需要辅助用额外的内存空间

## 算法复杂度

![Image](https://res.craft.do/user/preview/6fb64177-8f69-2661-4f82-68e681c36ecc/doc/993142F9-3B85-432E-92C9-9B20C518E1FF/C87F07E8-74A0-4596-89E7-9C64440A2F62_1)
## 初级排序 O(n^2)

### 选择排序 Selection Sort

每次找最小值，然后放到待排数组的而起始位置。

```js
function selectionSort(arr) {
  var len = arr.length
  var minIndex, temp
  for (var i = 0; i < len - 1; i++) {
	minIndex = i
	for (var j = i + 1; j < len; j++) {
	  if (arr[j] < arr[minIndex]) {
		// 寻找最小的数
		minIndex = j // 将最小数的索引保存
	  }
	}
	temp = arr[i]
	arr[i] = arr[minIndex]
	arr[minIndex] = temp
  }
  return arr
}
```

### 插入排序 Insertion Sort

从前到后逐步构建有序序列。对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

```js
function insertionSort(arr) {
  var len = arr.length;
  var preIndex, current;
  for(var i = 1; i < len; i++) {
	  preIndex = i - 1;
	  current = arr[i];
	  while(preIndex >= 0 && arr[preIndex] > current) {
		  arr[preIndex + 1] = arr[preIndex];
		  preIndex--;
	  }
	  arr[preIndex + 1] = current;
  }
  return arr;
}
```

### 冒泡排序 Bubble Sort

嵌套循环，每次查看相邻的元素，如果逆序，则交换。

```js
function bubbleSort(arr) {
  var len = arr.length
  for (var i = 0; i < len - 1; i++) {
	for (var j = 0; j < len - 1 - i; j++) {
	  if (arr[j] > arr[j + 1]) {
		// 相邻元素两两对比
		var temp = arr[j + 1] // 元素交换
		arr[j + 1] = arr[j]
		arr[j] = temp
	  }
	}
  }
  return arr
}
```

## 高级排序 O(nlogn)

### 快速排序 QuickSort

数组取标杆 pivot，将小元素放 pivot 左边，大元素放右侧，然后依次对右边和右边的子数组继续快排，以达到整个序列有序。 

```js
function quickSort(arr, left = 0, right = arr.length - 1) {
  if (arr.length <= 1) return arr;
  if (left < right) {
	const privot_index = partition(arr, left, right);
	quickSort(arr, left, privot_index - 1);
	quickSort(arr, privot_index + 1, right);
  }
}

function partition(arr, left, right) {
  let privot = left;
  let index = left + 1;
  for (let i = index; i <= right; i++) {
	if (arr[i] < arr[privot]) {
	  // 解构赋值
	  [arr[i], arr[index]] = [arr[index], arr[i]];
	  index++;
	}
  }
  [arr[privot], arr[index - 1]] = [arr[index - 1], arr[privot]];
  return index - 1;
}
```

### 归并排序 Merge Sort

算法描述：

- 把长度为n的输入序列分成两个长度为n/2的子序列；
- 对这两个子序列分别采用归并排序；
- 将两个排序好的子序列合并成一个最终的排序序列。
  
```js
function mergeSort(arr) {
  var len = arr.length
  if (len < 2) {
	return arr
  }
  var middle = Math.floor(len / 2),
	left = arr.slice(0, middle),
	right = arr.slice(middle)
  return merge(mergeSort(left), mergeSort(right))
}

function merge(left, right) {
  var result = []
  while (left.length > 0 && right.length > 0) {
	if (left[0] <= right[0]) {
	  result.push(left.shift())
	} else {
	  result.push(right.shift())
	}
  }
  while (left.length) result.push(left.shift())
  while (right.length) result.push(right.shift())
  return result
}
```

### 堆排序 Heap Sort

算法描述：

1. 数组元素依次简历小顶堆。
2. 依次取堆顶元素，并删除。
  
```js
var heapSort = function (arr, heapSize) {
  var i
  buildMaxHeap(arr, heapSize)
  for (i = heapSize - 1; i > 0; i--) {
	swap(arr, 0, i)
	maxHeapify(arr, 0, i)
  }
}

var buildMaxHeap = function (arr, heapSize) {
  var i,
	iParent = Math.floor((heapSize - 1) / 2)
  for (i = iParent; i >= 0; i--) {
	maxHeapify(arr, i, heapSize)
  }
}

var maxHeapify = function (arr, index, heapSize) {
  var iMax, iLeft, iRight
  do {
	iMax = index
	iLeft = 2 * index + 1
	iRight = 2 * (index + 1)

	// 是否左子节点比当前节点的值更大
	if (iLeft < heapSize && arr[index] < arr[iLeft]) {
	  iMax = iLeft
	}
	// 是否右子节点比当前更大节点的值更大
	if (iRight < heapSize && arr[iMax] < arr[iRight]) {
	  iMax = iRight
	}

	// 如果三者中，当前节点值不是最大的
	if (iMax != index) {
	  swap(arr, iMax, index)
	  index = iMax
	}
  } while (iMax != index)
}
var swap = function (arr, i, j) {
  var temp = arr[i]
  arr[i] = arr[j]
  arr[j] = temp
}

```

## 特殊排序 O(n)

### 计数排序 Counting Sort

计数排序要求输入的数据必须是有确定范围的整数。将输入的数据值转化为键存储在额外开辟的数组空间中吗，然后依次把计数大于1的填充回原数组。

```js
function countingSort(arr, maxValue) {
  var bucket = newArray(maxValue + 1),
	sortedIndex = 0,
	arrLen = arr.length,
	bucketLen = maxValue + 1

  for (var i = 0; i < arrLen; i++) {
	if (!bucket[arr[i]]) {
	  bucket[arr[i]] = 0
	}
	bucket[arr[i]]++
  }

  for (var j = 0; j < bucketLen; j++) {
	while (bucket[j] > 0) {
	  arr[sortedIndex++] = j
	  bucket[j]--
	}
  }

  return arr
}
```

### 桶排序 Bucket Sort

假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶再分别排序(有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排)。

```js
function bucketSort(arr, bucketSize = 5) {
  if (arr.length === 0) {
	return arr
  }
  var i
  var minValue = arr[0]
  var maxValue = arr[0]
  for (i = 1; i < arr.length; i++) {
	if (arr[i] < minValue) {
	  minValue = arr[i] // 输入数据的最小值
	} else if (arr[i] > maxValue) {
	  maxValue = arr[i] // 输入数据的最大值
	}
  }

  var bucketCount = Math.floor((maxValue - minValue) / bucketSize) + 1
  var buckets = new Array(bucketCount)
  for (i = 0; i < buckets.length; i++) {
	buckets[i] = []
  }

  //利用映射函数将数据分配到各个桶中
  for (i = 0; i < arr.length; i++) {
	buckets[Math.floor((arr[i] - minValue) / bucketSize)].push(arr[i])
  }

  arr.length = 0
  for (i = 0; i < buckets.length; i++) {
	insertionSort(buckets[i]) // 对每个桶进行排序，这里使用了插入排序
	for (var j = 0; j < buckets[i].length; j++) {
	  arr.push(buckets[i][j])
	}
  }

  return arr
}

function insertionSort(arr) {
  var len = arr.length;
  var preIndex, current;
  for(var i = 1; i < len; i++) {
	  preIndex = i - 1;
	  current = arr[i];
	  while(preIndex >= 0 && arr[preIndex] > current) {
		  arr[preIndex + 1] = arr[preIndex];
		  preIndex--;
	  }
	  arr[preIndex + 1] = current;
  }
  return arr;
}
```

### 基数排序 Radix Sort

基数排序是按照低位先排序，然后收集，再按照高位排序，然后再收集。依次类推，直到最高位。有时候有些属性是有优先级颀序的，先按低优先级排序,再按 高优先级排序。

```js
var counter = []
function radixSort(arr, maxDigit) {
  var mod = 10
  var dev = 1
  for (var i = 0; i < maxDigit; i++, dev *= 10, mod *= 10) {
	for (var j = 0; j < arr.length; j++) {
	  var bucket = parseInt((arr[j] % mod) / dev)
	  if (counter[bucket] == null) {
		counter[bucket] = []
	  }
	  counter[bucket].push(arr[j])
	}
	var pos = 0
	for (var j = 0; j < counter.length; j++) {
	  var value = null
	  if (counter[j] != null) {
		while ((value = counter[j].shift()) != null) {
		  arr[pos++] = value
		}
	  }
	}
  }
  return arr
}
```

## 参考资料
[十大经典排序算法（动图演示） - 一像素 - 博客园](https://www.cnblogs.com/onepixel/p/7674659.html)
[浅解前端必须掌握的算法（五）：堆排序（下）](https://juejin.cn/post/6844903633796988936)
