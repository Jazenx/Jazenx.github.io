---
title: Moves in squared strings (III) 
date: 2020-6-11 16:26:07
tags: Codewars
---

You are given a string of `n` lines, each substring being `n` characters long: For example:

`s = "abcd efgh ijkl mnop"`

We will study some transformations of this square of strings.

- Symmetry with respect to the main diagonal: diag*1*sym (or diag1Sym or diag-1-sym)
```
diag_1_sym(s) => "aeim\nbfjn\ncgko\ndhlp"
```

- Clockwise rotation 90 degrees: rot*90*clock (or rot90Clock or rot-90-clock)
```
rot_90_clock(s) => "miea\nnjfb\nokgc\nplhd"
```

- selfie*and*diag1(s) (or selfieAndDiag1 or selfie-and-diag1) It is initial string + string obtained by symmetry with respect to the main diagonal.
```
s = "abcd\nefgh\nijkl\nmnop" --> 
"abcd|aeim\nefgh|bfjn\nijkl|cgko\nmnop|dhlp"
```

#### Task:

- Write these functions `diag_1_sym`, `rot_90_clock`, `selfie_and_diag1`
- high-order function `oper(fct, s)` where
  - fct is the function of one variable f to apply to the string `s` (fct will be one of `diag_1_sym`, `rot_90_clock`, `selfie_and_diag1`)

## 解题

给定已个`/n`换行的，每行 n 个字节长，然后针对这个正方形做一些操作，例如：

`s = "abcd/nefgh/nijkl/nmnop"`

我画个图吧，比较形象。

![Image](https://res.craft.do/user/preview/6fb64177-8f69-2661-4f82-68e681c36ecc/doc/75C247F8-3AAB-4784-9884-7594750A58AC/809AE0BF-C402-416F-B801-0BC184F11A28_1)
现在针对这个正方形要写一些操作方法来返回新的字符串。

1. diag1Sym 需要相对于主对角线的对称性，如图：
![Image](https://res.craft.do/user/preview/6fb64177-8f69-2661-4f82-68e681c36ecc/doc/75C247F8-3AAB-4784-9884-7594750A58AC/062EA5FA-7BC8-400A-A7B7-F97F20DBB996_1)

```js
const diag1Sym = str => {
  const lineArr = str.split('\n')
  const n = lineArr.length
  let arr = []
  for(let i = 0; i < n; i+=1) {
    let line = []
    for (let j = 0; j < n; j+=1) {
      line.push(lineArr[j][i])
    }
	  arr[i] = line.join('')
  }
  return arr.join('\n')
}
```

2. 顺时针旋转90度
![Image](https://res.craft.do/user/preview/6fb64177-8f69-2661-4f82-68e681c36ecc/doc/75C247F8-3AAB-4784-9884-7594750A58AC/FF7C0033-1394-4AA6-ABBD-CB4AB45A1D53_1)

```js
const rot90Clock = str => {
  const lineArr = str.split('\n')
  const n = lineArr.length
  let arr = []
  for(let i = 0; i < n; i+=1) {
	let line = []
	for (let j = n - 1; j >= 0; j-=1) {
	  line.push(lineArr[j][i])
	}
	arr[i] = line.join('')
  }
  return arr.join('\n')
}
```

3. 初始字符串+相对于主对角线对称得到的字符串
   
```js
const selfieAndDiag1 = str => {
  const lineArr = str.split('\n')
  const n = lineArr.length
  let arr = []
  for(let i = 0; i < n; i+=1) {
	let line = []
	for (let j = 0; j < n; j+=1) {
	  line.push(lineArr[j][i])
	}
	arr[i] = `${lineArr[i]}|${line.join('')}`
  }
  return arr.join('\n')
}
```

写到最后尴尬了。

需要写一个高阶函数。之前三种操作作为参数。

暴力法，真的难顶。 发现也没有什么特别的逻辑好抽出来。

只能硬写过测例了。哈哈哈~

```js
function oper(fct, s) {
  return fct(s)
}
```

暴力法写代码，只能凭借直觉快速解题，对于该题思路没有什么问题，但是非常的不优雅，如果仔细考虑，会发现很多能复用的操作，没有抽取出来。我想要的代码是优雅和极简的，经过一番借鉴 solutions 和反复精简，最后的解题代码如下：

```js
const diag1Sym = s => s.map((v, x) => [...v].map((_, y)=> s[y][x]).join(''))
const rot90Clock = s => diag1Sym(s).map(v => [...v].reverse().join(''))
const selfieAndDiag1 = s => s.map((v, i) => `${v}|${diag1Sym(s)[i]}`)
const oper = (fct, s) => fct(s.split('\n')).join('\n')
```

## 参考资料

[Training on Moves in squared strings (III) | Codewars](https://www.codewars.com/kata/56dbeec613c2f63be4000be6/train/javascript)
