---
title: Valid Spacing
date: 2020-8-23 18:02:51
tags: Codewars
---
## Task

Your task is to write a function called `valid_spacing()` or `validSpacing()` which checks if a string has valid spacing. The function should return either `True` or `False`.

For this kata, the definition of valid spacing is one space between words, and no leading or trailing spaces. Below are some examples of what the function should return.

需要一个`validSpacing()`  来检查一个只有字母的字符串中的空格是否合法，返回 true 或者 false。

对于这个 kata，有效间距的定义是单词之间有一个空格，没有前导空格或尾随空格。下面是函数应该返回的一些示例：

```js
'Hello world' = true
' Hello world' = false
'Hello world  ' = false
'Hello  world' = false
'Hello' = true
// Even though there are no spaces, it is still valid because none are needed
'Helloworld' = true 
// true because we are not checking for the validity of words.
'Helloworld ' = false
' ' = false
'' = true
```

## Solution

首先可以通过  js 提供的 api 暴力解决下，我的思路是先判断首尾是否有空格，如果有空格直接返回 false，然后把字符串根据空格来拆分成数组，然后判断下数组里的元素是否有空格，如果有则返回 false， 当然还需要判断下边界条件来通过 case。代码如下：

```js
function validSpacing(s) {
  if(s === '') return true
  if(s.startsWith(' ') || s.endsWith(' ')) return false
  return !s.split(" ").some(w => w === '')
}
```

上面的代码。可读性尚可，但是操作步骤过于繁琐，判断条件过多。对于字符串处理，如果能用一个正则表单式来解决，那最好不过了。

我在 solutions 里看到一个解法：

思路是通过正则匹配字符串里的一个或多个空格，将匹配的结果替换成标准的一个空格，然后 trim() 下替换后字符串。此时得到的应该是标准合规的字符串，然后再与需要校验的字符串判断相等与否。比较骚。一行完成战斗。步骤也比我暴力写法少了几步。

```js
const validSpacing = (s) => s.replace(/\s+/g, " ").trim() === s;
```

那有没有一行正则解决战斗的呢？

有！

我们可以写一个正则去匹配非法的字符串。思路是

- 判断字符串是否以空格开始
- 判断是否存在以空格开始的非单词字符，即非法单词
- 判断字符串是否以空格结束

代码如下：

```js
function validSpacing(s) {
  return !(/^[\s]|([\s][\W])|[\s]$/g.test(s))
}
```

[Training on Valid Spacing | Codewars](https://www.codewars.com/kata/5f77d62851f6bc0033616bd8/train/javascript?collection&#x3D;strings-130)
